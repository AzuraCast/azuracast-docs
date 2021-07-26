---
title: Installation
description: How to install AzuraCast on your server
published: true
date: 2021-07-26T03:59:14.372Z
tags: getting started
editor: markdown
dateCreated: 2021-02-05T19:10:58.100Z
---

AzuraCast is server-based software that is installed on a VPS, dedicated server or other computer and serves both your station broadcasts and its own web interface through the server's network connection.

You can also install AzuraCast yourself on most server hardware and many computers running Linux.

> Installing AzuraCast yourself requires a basic understanding of the Linux shell/terminal environment, as well as root (or "sudo") access to the computer you're installing AzuraCast on.
{.is-info}


# One-Click Installers

The easiest way to get started with AzuraCast is to create a new VPS with one of our supported partners via the options below.

- **DigitalOcean:** Create a new droplet based on our one-click installer from the [AzuraCast listing on the DigitalOcean Marketplace](https://marketplace.digitalocean.com/apps/azuracast?refcode=1023fa8af513).

- **Linode:** Deploy a new Linode instance with the [AzuraCast one-click installer in the Linode Marketplace](https://www.linode.com/marketplace/apps/linode/azuracast/?r=68daf2976efcb77d2e3d4ced67a02b031edc3ba1).

# Self-Hosted Installation

## :whale: Docker (Recommended)

> For a majority of users, our Docker installation method is the preferred way of installing and using AzuraCast.
{.is-success}

Click the link below for Docker installation instructions.

- [Installing AzuraCast with Docker](/en/getting-started/installation/docker)
{.links-list}

## Other Methods

> Ansible installations are not officially supported; we strongly recommend Docker installations.
> 
> Ansible installations are an advanced option for seasoned Linux server administrators. The software we install can conflict with other software installed on your server and cause problems which are difficult to diagnose and support.
> 
> We do not provide support if you install AzuraCast on a server through the Ansible method.
{.is-warning}

Most servers and hosting providers support Docker without any issues. If your provider does not, or if you are using unique hardware, see the other guides below:

- [Installing AzuraCast on a Raspberry Pi 3B/4](/en/getting-started/installation/raspberry-pi)
- [Installing AzuraCast with Ansible](/en/getting-started/installation/ansible)
{.links-list}
