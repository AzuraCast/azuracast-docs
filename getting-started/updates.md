---
title: Updates
description: How to update your AzuraCast installation
published: true
date: 2021-02-08T05:00:51.896Z
tags: ansible, docker, getting started, debugging
editor: markdown
dateCreated: 2021-02-05T20:11:10.517Z
---

Updating AzuraCast will update both the web app itself and all of its dependencies, so you will be on the latest version of all of the supporting software.

During the update process, your stations will be briefly offline to listeners, so you should set aside a time to update and notify listeners if necessary.

# Docker Installations

Using the included Docker utility script, updating is as simple as running:

```
cd /var/azuracast
./docker.sh update-self
./docker.sh update
```

By default, the updater will prompt you to update your `docker-compose.yml` file. If you aren't making any changes to this file and want to automate the update process, you can use the command below to automatically answer "yes" to this question:

```
cd /var/azuracast
./docker.sh update-self
yes "" | ./docker.sh update
```

# Ansible

AzuraCast also includes a handy updater script for Ansible installations that pulls down the latest copy of the codebase from Git, flushes the site caches and makes any necessary database updates. Run these commands as any user with `sudo` permissions:

```
cd /var/azuracast/www
 
sudo chmod a+x update.sh
sudo ./update.sh
```

## Force a Full Update

Normally, the Ansible installer's update script only updates the portion of the system that have been modified since your last update. If an update was interrupted or otherwise is causing trouble, you can force the update script to process all components, which can often fix any issues:

```
./update.sh --full
```

# Switching Update Release Channels

AzuraCast ships two different release channels ("Stable" and "Rolling Release"), which you can switch between per installation. For more information, see our [Release Channels](/en/getting-started/updates/release-channels) page.