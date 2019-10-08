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
apt-get install fail2ban -y && \
sudo touch /etc/fail2ban/jail.local && \
sudo nano /etc/fail2ban/jail.local
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
sudo nano /etc/crontab
```


# NGINX
## NGINX? Why not...
Look, I'm going to use NGINX because it's [2.5x faster](https://www.eschrade.com/page/performance-of-apache-2-4-with-the-event-mpm-compared-to-nginx/) and was generally built out to handle more connections. This is Nextcloud installation is going to be a *file-sharing* site, so I need the throughput of uploading files as well as multiple people downloading files.

## Turn off Apache2



## NGINX Installation
