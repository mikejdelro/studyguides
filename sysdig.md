[tags]: <> (linux, sysdig, falco, security)

# Table of Contents
- [Table of Contents](#table-of-contents)
- [Rationale](#rationale)
- [Objectives](#objectives)
- [Requirements](#requirements)
- [Phases](#phases)
  - [I. VM Deploy and Secure](#i-vm-deploy-and-secure)
    - [VirtualBox](#virtualbox)
    - [Debian](#debian)
    - [Installing Debian on VirtualBox](#installing-debian-on-virtualbox)
      - [Downloading the ISO](#downloading-the-iso)
      - [Installing on VirtualBox](#installing-on-virtualbox)
    - [Updating Debian](#updating-debian)
    - [Securing Debian](#securing-debian)
  - [II. Kubernetes](#ii-kubernetes)
    - [What is Kubernetes](#what-is-kubernetes)
    - [](#)
  - [III. Deploy Kubernetes](#iii-deploy-kubernetes)
  - [IV. Automate Deploy](#iv-automate-deploy)
  - [V. LAMP Stack](#v-lamp-stack)
  - [VI. Deploy LAMP Stack in Kubernetes](#vi-deploy-lamp-stack-in-kubernetes)
  - [VII. Deploy Basic Web Service in LAMP Stack](#vii-deploy-basic-web-service-in-lamp-stack)
  - [VIII. Automate LAMP Stack deploy](#viii-automate-lamp-stack-deploy)
  - [IX. SysDig Falco](#ix-sysdig-falco)
  - [X. Deploy SysDig Falco](#x-deploy-sysdig-falco)
  - [XI. Learn how to Parse SysDig Falco Output](#xi-learn-how-to-parse-sysdig-falco-output)
  - [XII. Feed SysDig Falco Output to Web Service](#xii-feed-sysdig-falco-output-to-web-service)
  - [XIII. Automate SysDig Falco Deploy](#xiii-automate-sysdig-falco-deploy)
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
3. Time.

# Phases

## I. VM Deploy and Secure

### VirtualBox
Locally, this guide will try and use an instance of [VirtualBox](https://www.virtualbox.org/) to deploy an instance of Debian.

### Debian

For reference, I am already comfortable in deployment and the data structure of Debian-flavored Linux (*buntu and Kali), so, I'll make use of that familiarity to deploy these tools.

### Installing Debian on VirtualBox

#### Downloading the ISO

#### Installing on VirtualBox

### Updating Debian

### Securing Debian

## II. Kubernetes

### What is Kubernetes

### 

## III. Deploy Kubernetes

## IV. Automate Deploy

## V. LAMP Stack

## VI. Deploy LAMP Stack in Kubernetes

## VII. Deploy Basic Web Service in LAMP Stack

## VIII. Automate LAMP Stack deploy

## IX. SysDig Falco

## X. Deploy SysDig Falco

## XI. Learn how to Parse SysDig Falco Output

## XII. Feed SysDig Falco Output to Web Service

## XIII. Automate SysDig Falco Deploy

# References