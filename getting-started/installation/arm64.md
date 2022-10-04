---
title: Installing AzuraCast on ARM64 Computers
description: 
published: true
date: 2021-12-14T07:41:06.431Z
tags: ansible, getting started
editor: markdown
dateCreated: 2021-02-05T19:54:24.611Z
---

AzuraCast is compatible with any device or server capable of running the ARM64 architecture, including the Raspberry Pi 3, 3B, 4, as well as a broad variety of competing single-board computers.

Keep in mind that several of these ARM-based computers are provisioned with very limited system resources, and as such they may not be ideal for running a production AzuraCast instance. They can, however, be an excellent place to run a smaller station, to host a relay, or for local testing.

## Docker Installation Support

As of our latest Rolling Release version, AzuraCast now builds Docker images that support the ARM64 architecture. As long as you are using an ARM device and operating system that supports this architecture, you can use our standard Docker installation without any issues.

- [Docker Installation](/en/getting-started/installation/docker)
{.links-list}

## Ansible Installation Support

On some lower-powered ARM64 devices, like the Raspberry Pi 3 and 3B, it can be preferable to use the Ansible installation method, as this method can make better use of limited resources.

Keep in mind that installing with Ansible can introduce some unexpected problems, and requires a stronger understanding of Linux fundamentals than our Docker installation.

### Installing Ubuntu

If using a web host that offers ARM-based hosts (such as Scaleway), select an ARM64 device and choose the Ubuntu 20.04 (Focal Fossa) distribution. This will allow for the fastest installation method.

If you're using a Raspberry Pi device, visit the [Ubuntu Raspberry Pi download page](https://ubuntu.com/download/raspberry-pi), where they host a variety of images that are ready to use on these devices. **Make sure that you select both the 64-bit and Long-Term Support (LTS) version of Ubuntu** that is right for your device.

### Set up Swap Space

If your ARM64 device has less than 4GB of RAM available, it is recommended to [create a swap space](https://linuxize.com/post/how-to-add-swap-space-on-ubuntu-20-04/) before running the AzuraCast installation to avoid issues with running out of memory.

### Running the Ansible Installation

Once the operating system is booted up and you have connected to the device either via SSH or by directly accessing the terminal, you can run the standard [Ansible installation instructions](/en/getting-started/installation/ansible) without any changes.