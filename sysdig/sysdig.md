
[tags]: <> (linux, sysdig, falco, security)

# Table of Contents
- [Table of Contents](#table-of-contents)
- [Rationale](#rationale)
- [Objectives](#objectives)
- [Requirements](#requirements)
- [Phases](#phases)
  - [Docker](#docker)
    - [What is Docker](#what-is-docker)
    - [Installing Docker in OSX](#installing-docker-in-osx)
  - [LEMR](#lemr)
    - [What is LEMR](#what-is-lemr)
    - [NGINX](#nginx)
    - [Configuring NGINX](#configuring-nginx)
    - [MySQL](#mysql)
    - [Configuring MySQL](#configuring-mysql)
    - [Rails](#rails)
    - [Configuring Rails](#configuring-rails)
  - [Deploy LEMR Stack in Docker](#deploy-lemr-stack-in-docker)
  - [SysDig Falco](#sysdig-falco)
  - [Deploy SysDig Falco](#deploy-sysdig-falco)
  - [Create sysdig falco rules](#create-sysdig-falco-rules)
  - [Kubernetes](#kubernetes)
    - [What is Kubernetes](#what-is-kubernetes)
  - [Deploy Kubernetes](#deploy-kubernetes)
  - [Automate Deploy](#automate-deploy)
  - [Deploy LEMR/railsgoat on kubernetes](#deploy-lemrrailsgoat-on-kubernetes)
  - [Deploy sysdig falco](#deploy-sysdig-falco)
  - [Deploy rules on minikube](#deploy-rules-on-minikube)
  - [Learn how to Parse SysDig Falco Output](#learn-how-to-parse-sysdig-falco-output)
  - [Kibana](#kibana)
  - [Feed SysDig Falco Output to Kibana](#feed-sysdig-falco-output-to-kibana)
- [References](#references)

# Rationale

This guide aims to standardize knowledge capture by creating a linear process of progression with the end goal of deploying and studying SysDig Falco. Each individiual step will document and log learned processes (all processes done, either successful or erroneous) and polish/optimize these said processes.

# Objectives

At the end of documentation, any developer who follows this guide should have or should be able to:

1. Deploy a networked VM with WAN/LAN access.
2. Secure said VM using conventional means.
3. Deploy Kubernetes on said VM.
4. Deploy a basic web service in Kubernetes.
5. Deploy SysDig Falco, monitoring the container for the web service.
6. Gather and interpret logs provided by SysDig Falco.
7. Feed the gathered logs of SysDig Falco to an aggregator web service.
8. Automate all known processes in either scripts or optimized processes.

# Requirements

1. Local networked machine that you are able to deploy a VM on, or a purchased VPS KVM commercially available on the internet.
2. Prior knowledge of Linux technology, such as the Linux file directory, `git`, `yum`, text editors, etc.

# Phases

## Docker

### What is Docker

### Installing Docker in OSX

## LEMR 

### What is LEMR

### NGINX

### Configuring NGINX

### MySQL

### Configuring MySQL

### Rails

### Configuring Rails

## Deploy LEMR Stack in Docker

## SysDig Falco

## Deploy SysDig Falco

## Create sysdig falco rules

## Kubernetes

### What is Kubernetes

## Deploy Kubernetes

## Automate Deploy

## Deploy LEMR/railsgoat on kubernetes

## Deploy sysdig falco

## Deploy rules on minikube

## Learn how to Parse SysDig Falco Output

## Kibana

## Feed SysDig Falco Output to Kibana

# References