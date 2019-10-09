Okay, so this gets tedious whenever I try to deploy it, as I always seem to end up hitting a wall on something or another when it goes live.

# Server Preparation

## Getting Ready
```bash
apt-get update -y && \
apt-get install sudo nano curl software-properties-common -y && \
sudo locale-gen en_US.UTF-8
```
Easy peasy, starting it off simple with `apt-get update` by updating the package database in our VPS as well as any old packages. 

`sudo` to make sure we can handle Super User access easily.

`nano` because I'm a pleb and this is a quick text editor that I know how to use.

`curl` to pull specific commands from web-addresses.

The `software-properties-common` command will install some commonly used software for `apt`, which manages your software vendor sources such as `locale-gen`.

The `locale-gen` command sets the working language for the bash terminal as well as the commands to English. This may become problematic if your terminal is running one language and your VPS is using another. 
You'll end up seeing a bunch of errors like this:

```bash
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
    LANGUAGE = (unset),
    LC_ALL = (unset),
    LANG = "en_US.UTF-8"
are supported and installed on your system.
```

> I did not `apt-get upgrade` as that, unfortunately, usually breaks an OpenVZ VPS.

## Server Security

```bash
apt-get install fail2ban -y && sudo nano /etc/fail2ban/jail.local
```
Okay, our first configuration, but before we dive in, an explanation.

### Fail2Ban

So, [Wikipedia](https://en.wikipedia.org/wiki/Fail2ban) says that Fail2Ban is an intrusion prevention software framework that protects computer servers from brute-force attacks. It does that by monitoring log files (e.g. `/var/log/auth.log`, `/var/log/apache/access.log`, etc.) for selected entries and running scripts based on them. 

Most commonly, Fail2Ban is used to block selected IP addresses that may belong to hosts that are trying to breach the system's security. It can ban any host IP address that makes too many login attempts or performs any other unwanted action within a time frame defined by the administrator.

The absolute worst thing about OpenVZ VPS is it can, without anyone knowing, be easily turned into a [hacker's zombie](https://en.wikipedia.org/wiki/Zombie_(computing)).

### Securing SSH
Assuming that SSH is the only way to access your site, we can lock it down by pasting this section to `/etc/fail2ban/jail.local` (which, if you pasted the whole command up there in entirety, should open it for you to edit)

```bash
[sshd]
enabled = true
banaction = iptables-multiport
maxretry = 3
findtime = 43200
bantime = 86400
```
[Source for this snippet](https://www.booleanworld.com/protecting-ssh-fail2ban/) which also explains what each command does.

### Enabling Fail2Ban

```bash
sudo systemctl enable fail2ban && sudo systemctl restart fail2ban
```
Finally, the `enable` command should allow fail2ban to initialize with your system and `restart` will reboot fail2ban and apply your jail settings.

## Scheduled Reboots
```bash
sudo crontab -e
```

```
0 4   *   *   *    /sbin/shutdown -r +5
```

# NGINX
## NGINX? Why not...
Look, I'm going to use NGINX because it's [2.5x faster](https://www.eschrade.com/page/performance-of-apache-2-4-with-the-event-mpm-compared-to-nginx/) and was generally built out to handle more connections. This is Nextcloud installation is going to be a *file-sharing* site, so I need the throughput of uploading files as well as multiple people downloading files.

## Turn off Apache2
```bash
sudo service apache2 stop
sudo apt remove apache2.*
sudo apt autoremove
sudo apt-get remove apache2*
```



## NGINX Installation
```bash
sudo apt install nginx
```

```bash
sudo systemctl enable nginx && sudo systemctl restart nginx
```

## MariaDB installation
```bash
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository "deb [arch=amd64,arm64,ppc64el] http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu $(lsb_release -cs) main"
sudo apt update && sudo apt install mariadb-server mariadb-client -y
```

```bash
sudo systemctl enable mysql && sudo systemctl restart mysql && sudo mysql_secure_installation
```

## PHP7.2
```bash
sudo add-apt-repository ppa:ondrej/php -y && sudo apt update
```

```bash
sudo apt install php7.2 php7.2-fpm php7.2-mysql php-common php7.2-cli php7.2-common php7.2-json php7.2-opcache php7.2-readline php7.2-mbstring php7.2-xml php7.2-gd php7.2-curl php7.2-intl php7.2-imagick php7.2-zip -y
```

```bash
sudo systemctl enable php7.2-fpm && sudo systemctl restart php7.2-fpm
```

## Nextcloud
```bash
wget https://download.nextcloud.com/server/releases/latest.zip && unzip latest.zip && sudo mv nextcloud /usr/share/nginx/ && sudo chown www-data:www-data /usr/share/nginx/nextcloud/ -R
```

## MariaDB setup
```bash
sudo mysql -u root
```

```sql
create database nextcloud;
create user nextclouduser@localhost identified by 'h@s4p@55w0rD';
grant all privileges on nextcloud.* to nextclouduser@localhost identified by 'h@s4p@55w0rD';
flush privileges;
exit;
```

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

```bash
innodb_file_per_table=1
log-bin        = /var/log/mysql/mariadb-bin
log-bin-index  = /var/log/mysql/mariadb-bin.index
binlog_format  = mixed
```

```bash
sudo systemctl restart mysql
```

## NGINX setup

```bash
server {
    listen 80;
    server_name nextcloud-url;

    # Add headers to serve security related headers
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Robots-Tag none;
    add_header X-Download-Options noopen;
    add_header X-Permitted-Cross-Domain-Policies none;

    # Path to the root of your installation
    root /usr/share/nginx/nextcloud/;

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    # The following 2 rules are only needed for the user_webfinger app.
    # Uncomment it if you're planning to use this app.
    #rewrite ^/.well-known/host-meta /public.php?service=host-meta last;
    #rewrite ^/.well-known/host-meta.json /public.php?service=host-meta-json
    # last;

    location = /.well-known/carddav {
        return 301 $scheme://$host/remote.php/dav;
    }
    location = /.well-known/caldav {
       return 301 $scheme://$host/remote.php/dav;
    }

    location ~ /.well-known/acme-challenge {
      allow all;
    }

    # set max upload size
    client_max_body_size 512M;
    fastcgi_buffers 64 4K;

    # Disable gzip to avoid the removal of the ETag header
    gzip off;

    # Uncomment if your server is build with the ngx_pagespeed module
    # This module is currently not supported.
    #pagespeed off;

    error_page 403 /core/templates/403.php;
    error_page 404 /core/templates/404.php;

    location / {
       rewrite ^ /index.php$uri;
    }

    location ~ ^/(?:build|tests|config|lib|3rdparty|templates|data)/ {
       deny all;
    }
    location ~ ^/(?:\.|autotest|occ|issue|indie|db_|console) {
       deny all;
     }

    location ~ ^/(?:index|remote|public|cron|core/ajax/update|status|ocs/v[12]|updater/.+|ocs-provider/.+|core/templates/40[34])\.php(?:$|/) {
       include fastcgi_params;
       fastcgi_split_path_info ^(.+\.php)(/.*)$;
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       fastcgi_param PATH_INFO $fastcgi_path_info;
       #Avoid sending the security headers twice
       fastcgi_param modHeadersAvailable true;
       fastcgi_param front_controller_active true;
       fastcgi_pass unix:/run/php/php7.2-fpm.sock;
       fastcgi_intercept_errors on;
       fastcgi_request_buffering off;
    }

    location ~ ^/(?:updater|ocs-provider)(?:$|/) {
       try_files $uri/ =404;
       index index.php;
    }

    # Adding the cache control header for js and css files
    # Make sure it is BELOW the PHP block
    location ~* \.(?:css|js)$ {
        try_files $uri /index.php$uri$is_args$args;
        add_header Cache-Control "public, max-age=7200";
        # Add headers to serve security related headers (It is intended to
        # have those duplicated to the ones above)
        add_header X-Content-Type-Options nosniff;
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Robots-Tag none;
        add_header X-Download-Options noopen;
        add_header X-Permitted-Cross-Domain-Policies none;
        # Optional: Don't log access to assets
        access_log off;
   }

   location ~* \.(?:svg|gif|png|html|ttf|woff|ico|jpg|jpeg)$ {
        try_files $uri /index.php$uri$is_args$args;
        # Optional: Don't log access to other assets
        access_log off;
   }
}
```

```bash
sudo systemctl reload nginx
```

## Setup SSL

```bash
sudo add-apt-repository ppa:certbot/certbot && sudo apt update && sudo apt install certbot python-certbot-nginx -y
```

```bash
sudo certbot --nginx --agree-tos --redirect --staple-ocsp --email your-email -d your-url
```

```bash
sudo mkdir /usr/share/nginx/nextcloud-data && sudo chown www-data:www-data /usr/share/nginx/nextcloud-data -R
```

```bash
sudo nano /etc/php/7.2/fpm/pool.d/www.conf

;env[HOSTNAME] = $HOSTNAME
;env[PATH] = /usr/local/bin:/usr/bin:/bin
;env[TMP] = /tmp
;env[TMPDIR] = /tmp
;env[TEMP] = /tmp

sudo systemctl reload php7.2-fpm
```

```bash
sudo nano /etc/php/7.2/fpm/php.ini

memory_limit = 1024M

sudo systemctl reload php7.2-fpm
```

```bash
sudo nano /etc/php/7.2/fpm/pool.d/www.conf

pm = dynamic
pm.max_children = 120
pm.start_servers = 12
pm.min_spare_servers = 6
pm.max_spare_servers = 18

sudo systemctl reload php7.2-fpm
```