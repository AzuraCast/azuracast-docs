---
title: Requirements
description: This page tells you what is required to run AzuraCast
published: true
date: 2021-02-06T22:24:35.811Z
tags: 
editor: markdown
dateCreated: 2021-02-05T18:58:13.221Z
---

AzuraCast is powered by Docker and uses pre-built images that contain every component of the software. Don't worry if you aren't very familiar with Docker; our easy installer tools will handle installing Docker and Docker Compose for you, and updates are very simple.


# System Requirements

- A 64-bit x86 (x86_64) CPU
- at least 2GB of RAM
- 20GB or greater of hard drive space
- A computer/server capable of running Docker

> For Linux hosts, the `sudo`, `curl` and `git` packages should be installed before installing AzuraCast. Most Linux distributions include these packages already.
{.is-info}

# Operating System

> Regardless of which OS you choose, to avoid any compatibility or permissions issues you should install AzuraCast on a fresh minimal installation without Docker or Docker Compose installed.
{.is-success}

We strongly recommend one of the following distributions and versions for your AzuraCast installation:

- Ubuntu 20.04 LTS (Recommended)
- Ubuntu 18.04 LTS
- Ubuntu 16.04 LTS (Compatible, but updating is recommended)
- Debian 10 "Buster"
- Debian 9 "Stretch"

## Known Incompatibilities

<br>

### **OpenVZ or LXC-based Web Hosts:**

Some hosting providers use OpenVZ or LXC, and sometimes these technologies are incompatible with Docker. If the Docker installation does not work on your host, you should consider using a different server for AzuraCast, or you can use the unsupported Ansible installation method.

<br>

### **CentOS:**
Recent versions of CentOS are moving away from supporting Docker and instead supporting Podman, a different container-based solution. AzuraCast does not currently support being deployed on Podman. If you are selecting a Linux OS for your server, the LTS distributions of Ubuntu and Debian are strongly recommended.

# Resource Usage

On the Administration page you can find a basic `Server Status` section that shows you the `CPU Load`, `Memory` and `Disk Space` usage of your server.

<br> 

## IceCast

For IceCast you can find some benchmarks at https://icecast.org/loadtest/

