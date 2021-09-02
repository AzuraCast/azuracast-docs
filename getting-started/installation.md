---
title: Installation
description: How to install AzuraCast on your server
published: true
date: 2021-09-02T02:25:42.934Z
tags: getting started
editor: markdown
dateCreated: 2021-02-05T19:10:58.100Z
---

AzuraCast is server-based software that is installed on a VPS, dedicated server or other computer and serves both your station broadcasts and its own web interface through the server's network connection.

You can also install AzuraCast yourself on most server hardware and many computers running Linux.

# One-Click Installers

The easiest way to get started with AzuraCast is to create a new VPS with one of our supported partners via the options below.

## One-Click Installers {.tabset}
### DigitalOcean

Create a new droplet based on our one-click installer from the AzuraCast listing on the DigitalOcean Marketplace.

- [DigitalOcean AzuraCast One-Click Image](https://marketplace.digitalocean.com/apps/azuracast?refcode=21612b90440f)
{.links-list}

### Linode

Deploy a new Linode instance with the AzuraCast one-click installer in the Linode Marketplace.

- [Linode AzuraCast One-Click Image](https://www.linode.com/marketplace/apps/linode/azuracast/?r=68daf2976efcb77d2e3d4ced67a02b031edc3ba1)
{.links-list}

# Self-Hosted Installation

> Installing AzuraCast yourself requires a basic understanding of the Linux shell/terminal environment, as well as root (or "sudo") access to the computer you're installing AzuraCast on.
{.is-info}

> **Want professional help?** 
> AzuraCast offers paid [professional services](/en/professional-services) to help with installing, updating, migrating or maintaining your AzuraCast installation.
{.is-info}

## Installation Methods {.tabset}
### :whale: Docker (Recommended)

For a majority of users, our Docker installation method is the preferred way of installing and using AzuraCast. Click the link below for Docker installation instructions.

- [Installing AzuraCast with Docker](/en/getting-started/installation/docker)
{.links-list}


### Ansible

Ansible installations are intended for seasoned Linux server administrators and are not recommended or officially supported for most users; we strongly recommend Docker installations in a majority of cases. The software we install via Ansible can conflict with other software installed on your server and cause problems which are difficult to diagnose and support. You should be familiar with the Linux terminal and with basic troubleshooting and diagnostic steps if you choose this installation method.

- [Installing AzuraCast with Ansible](/en/getting-started/installation/ansible)
{.links-list}

### Synology NAS

Installing AzuraCast on a Synology NAS involves specific steps, so we've created a unique guide for installation on this platform.

Note: This guide is intended for X86/AMD64-based Synology NAS devices only. Devices running ARM processors are not officially supported.

- [Installing AzuraCast on a Synology NAS](/en/getting-started/installation/synology)
{.links-list}

### Raspberry Pi

Because Raspberry Pi devices use ARM processors, they currently cannot take advantage of our primary Docker installation method, and require specific setup steps to ensure successful operation. See the guide below for instructions on how to install AzuraCast on a Raspberry Pi 3B or 4 series.

- [Installing AzuraCast on a Raspberry Pi 3B or 4 Series](/en/getting-started/installation/raspberry-pi)
{.links-list}
