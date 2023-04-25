---
title: Docker Installation Guide
description: How to install AzuraCast with Docker
published: 1
date: 2023-04-25T05:14:05.223Z
tags: docker, getting started
editor: markdown
dateCreated: 2022-10-04T18:55:22.502Z
---

AzuraCast is powered by Docker and uses pre-built images that contain every component of the software. Don't worry if you aren't very familiar with Docker; our easy installer tools will handle installing Docker and Docker Compose for you, and updates are very simple.

# System Requirements

Please check the page with the [AzuraCast system requirements](/en/getting-started/requirements) before installing to make sure your server meets the minimum requirements.

# Installing

Connect to the server or computer you want to install AzuraCast on via an SSH terminal. You should be an administrator user with either root access or the ability to use the `sudo` command. If you are not the root user, you may need to run `sudo su -` before executing the commands below.

Pick a base directory on your host computer that AzuraCast can use. If you're on Linux, you can follow the steps below to use the recommended directory:

```
mkdir -p /var/azuracast
cd /var/azuracast
```

Use these commands to download our Docker Utility Script, set it as executable and then run the Docker installation process:

```
curl -fsSL https://raw.githubusercontent.com/AzuraCast/AzuraCast/main/docker.sh > docker.sh
chmod a+x docker.sh
./docker.sh install
```

On-screen prompts will show you how the installation is progressing.

> Once installation has completed, be sure to follow the [post-installation steps](/en/getting-started/installation/post-installation-steps).
{.is-success}


> You can also set up SSL with [Let's Encrypt](/en/administration/ssl-and-lets-encrypt) or make other changes to your installation using the Docker Utility Script that you've just downloaded
{.is-info}

## Troubleshooting

Some Virtual Private Server (VPS) providers sometimes ship a old version of Docker or Docker-Compose with their systems and the version isn't fully compatible with our installation script. This can be resolved by running these commands then attempting the installation process again. 
``` bash
cd /var/azuracast
./docker.sh install-docker
./docker.sh install-docker-compose
./docker.sh install
```

# Unattended Installation
To automate the installation process for AzuraCast, you can run the following commands or place them into a `cloud-init` script. You can select between release channels when installing:

## Rolling Release Channel

To automatically install AzuraCast on the rolling release channel, run this script:

```
mkdir -p /var/azuracast
cd /var/azuracast
curl -fsSL https://raw.githubusercontent.com/AzuraCast/AzuraCast/main/docker.sh > docker.sh
chmod a+x docker.sh
yes '' | ./docker.sh install
```

## Stable Channel

To automatically install AzuraCast on the stable channel, run this script:

```
mkdir -p /var/azuracast
cd /var/azuracast
curl -fsSL https://raw.githubusercontent.com/AzuraCast/AzuraCast/main/docker.sh > docker.sh
chmod a+x docker.sh
yes 'Y' | ./docker.sh setup-release
yes '' | ./docker.sh install
```

# After Installation

Once you've installed AzuraCast, you're ready to use our web interface to set up your administrator account, create your first station and customize the system settings.

Follow our [Post-Installation Steps](/en/getting-started/installation/post-installation-steps) for more details.

If you want to optimize your installation to get the most out of a server with limited resources, follow our [Optimizing](/en/getting-started/installation/optimizing) guide.