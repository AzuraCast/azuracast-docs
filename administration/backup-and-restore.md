---
title: Backup & Restore
description: Creating backups and restoring your AzuraCast installation from them
published: true
date: 2021-06-04T20:16:40.241Z
tags: administration
editor: markdown
dateCreated: 2021-02-06T07:01:08.655Z
---

# Backing Up

<br>

## Via the Web Interface

The "System Administration" section of your AzuraCast web interface has a dedicated "Backup" page, where you can configure periodic automated backups, run a one-time backup and download existing backup files.

Backups run through the web panel do not interrupt your broadcasts or disconnect your listeners.

## Via the Command Line

If you want more control over where your backup files are going, or want to take advantage of pre-existing tools (like cron tasks) to handle the backup, you can use the Docker Utility Tool to generate backups as well.

The most basic version of this command is:

```bash
cd /var/azuracast
./docker.sh backup path-to-backup.zip
```

You can also pass the --exclude-media flag to back up just the database and statistics, but not the media itself, which significantly reduces the backup file size (but you should make sure to back your media up elsewhere):

```bash
cd /var/azuracast
./docker.sh backup path-to-backup.zip --exclude-media
```

Both .zip and .tar.gz formats are supported for backups. The correct format will automatically be determined by the extension of the filename you specify for the backup file.

# Restoring a Backup

When you need to restore a backup of your AzuraCast installation you will first need to install a clean AzuraCast on your server.

See the documentation about [installing AzuraCast](/en/getting-started/installation).

> If you have an older backup file and encounter issues with restoring directly to the current rolling-release version you should install AzuraCast on the `stable` version first before restoring.
{.is-info}

After your installation is done you can start restoring from your backup by uploading the `.zip` file to the server and run the following command (replace `path-to-backup.zip` with the path to the `.zip` file):

```bash
cd /var/azuracast
./docker.sh restore path-to-backup.zip
```

> If you have installed AzuraCast on `stable` due to an old backup (as mentioned above) you should now switch AzuraCast back to `rolling-release`. See the guide for [switching release channels](https://docs.azuracast.com/en/getting-started/updates/release-channels#setting-azuracast-to-use-a-different-channel).
{.is-info}

After the restore process is done you should run the update command to make sure your installation is up-to-date:

```bash
cd /var/azuracast
./docker.sh update
```