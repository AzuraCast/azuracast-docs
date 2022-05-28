---
title: Installing AzuraCast with Ansible
description: 
published: true
date: 2022-05-28T05:01:15.018Z
tags: advanced feature, ansible
editor: markdown
dateCreated: 2021-02-05T19:53:05.881Z
---

> **We do not recommend this installation method for most users.** See the "Disadvantages" section below for more information. A greater familiarity with the Linux shell is required to use this installation method, and we don't provide support for many Ansible-specific issues. For most users, we strongly recommend the Docker installation method.
{.is-warning}

The Ubuntu Ansible installation method (formerly known as the "bare-metal" or "traditional" installation) is an advanced option available for those who want more complex customization options or are running on very limited hardware that can't handle the minor overhead of the Docker installation method.

## Ansible vs. Docker

Many users who are unfamiliar with server-side software are also unfamiliar with Docker, and are thus reluctant to use it. There are also valid reasons to use the Ansible installation in certain circumstances, which is why we continue to maintain it.

When evaluating which installation method to use, be aware of the following considerations in favor of, and against, using Ansible over our recommended Docker installation method.

### Advantages of Ansible

- **Advanced Customization:** For "power users" looking to build custom solutions, you can customize files and configuration more easily with Ansible installations. (Note that the Ansible updater may overwrite settings if necessary.)

- **Optimized for Low-Resource Computers:** Some computers that operate on narrow performance margins, like the Raspberry Pi devices, can benefit from AzuraCast running directly on the OS, rather than through Docker.

### Disadvantages of Ansible

- **Limited Operating System Selection:** This installation method requires using one of our few directly supported OS versions, namely the latest LTS releases of Ubuntu. With Docker, our software is configured inside containerized images, so you can use any Docker-supported host operating system.

- **"Clean" Installation Required:** The Ansible installation does not "coexist" with other software on an Ubuntu installation, and will often overwrite or conflict with other software. Docker's more "containerized" nature means you have a far lower chance of other software conflicting with AzuraCast.

- **Potential for Software Conflicts:** Because Ansible installs directly onto whatever version of Ubuntu you're running, it can't always take into account the specific configuration of your OS, or any bundled software that your hosting provider includes with it. This increases the likelihood that our installer may conflict with 

- **Longer Update Times:** If you use Docker, you can take advantage of the fact that our automated systems build the images for you, and all you need to do is pull down the latest images when updating. With Ansible, you have to run every updated installation step on your own computer, which can (and often does) take much longer.

- **No Built-in Automatic LetsEncrypt Support**

## Supported Operating Systems

Currently, the following operating systems are supported:

- **Ubuntu 20.04 "Focal" LTS (Recommended)**

Our support for LTS versions of Ubuntu follows the [Ubuntu support lifecycle](https://ubuntu.com/about/release-cycle). Typically, LTS versions are supported for a period of 5 years following their release.

> Some web hosts offer custom versions of Ubuntu that include different software repositories. These may cause compatibility issues with AzuraCast. Many VPS providers are known to work out of the box with AzuraCast (OVH, DigitalOcean, Vultr, etc), and are thus highly recommended if you plan to use the Bare-metal installer.
{.is-info}

AzuraCast is optimized for speed and performance, and can run on very inexpensive hardware, from the Raspberry Pi 3 to the lowest-level VPSes offered by most providers.

Since AzuraCast installs its own radio tools, databases and web servers, you should always install AzuraCast on a "clean" server instance with no other web or radio software installed previously.

## Installing

Execute these commands as a user with sudo permissions (or root) to set up your AzuraCast server:

```bash
sudo apt-get update 
sudo apt-get install -q -y git 
 
sudo mkdir -p /var/azuracast/www 
cd /var/azuracast/www 
sudo git clone https://github.com/AzuraCast/AzuraCast.git . 

# Want only stable "release" builds? Run this code here:
# git checkout -q -f stable
 
sudo chmod a+x install.sh 
./install.sh
```

The installation process will take between 5 and 15 minutes, depending on your Internet connection.

Once the terminal-based installation is complete, follow the [post-installation steps](/en/getting-started/installation/post-installation-steps).