---
title: Synchronisation Tasks
description: All about the AzuraCast automatic processing tasks
published: true
date: 2021-04-17T10:52:27.198Z
tags: administration
editor: markdown
dateCreated: 2021-02-06T04:53:24.798Z
---

AzuraCast has some background tasks that are regularly triggered to run internal processing jobs that are neccessary for the operation of your stations.

On this page you will find an overview of all the `Sync Tasks` that AzuraCast depends on.

# Sync Times

There are 4 sync times defined in AzuraCast. Each `Sync Task` is assigned to one of these times and will be run accordingly.

Name | Time Interval | Sync Tasks
- | - | - 
Now Playing Data | 15 seconds | `BuildQueueTask`, `NowPlayingTask`, `ReactivateStreamerTask`
1-Minute Sync | 60 seconds | `CheckRequests`, `RunBackupTask`, `CleanupRelaysTask`
5-Minute Sync | 5 minutes | `CheckMediaTask`, `CheckFolderPlaylistsTask`, `CheckUpdatesTask`
1-Hour Sync | 60 minutes | `RunAnalyticsTask`, `RunAutomatedAssignmentTask`, `CleanupHistoryTask`, `CleanupStorageTask`, `RotateLogsTask`, `UpdateGeoLiteTask`

<br>

# Increase Execution Timeout

If you are regularly running into timeouts while a sync task is processing you can manually increase the time that the sync tasks are allowed to run via the `azuracast.env` in `/var/azuracast/`.

In the `azuracast.env` look for the following entries and increase the specified time in seconds to your needs.

```
# The maximum execution time (and lock timeout) for the 15-second, 1-minute and 5-minute synchronization tasks.
# SYNC_SHORT_EXECUTION_TIME=600

# The maximum execution time (and lock timeout) for the 1-hour synchronization task.
# SYNC_LONG_EXECUTION_TIME=1800
```

This is how these lines should look like after changing (the values are just for demonstration):

```
# The maximum execution time (and lock timeout) for the 15-second, 1-minute and 5-minute synchronization tasks.
SYNC_SHORT_EXECUTION_TIME=900

# The maximum execution time (and lock timeout) for the 1-hour synchronization task.
SYNC_LONG_EXECUTION_TIME=2200
```

After changing these entries restart your AzuraCast installation via:

```
docker-compose down
docker-compose up -d
```

# Now Playing Data

<br>

## BuildQueueTask

This task is responsible for automatically building the `Upcoming Songs Queue` for the AutoDJ.

## NowPlayingTask

This task is responsible for updating the now playing data of AzuraCast from the information that is available from IceCast, SHOUTcast and Liquidsoap.

## ReactivateStreamerTask

When manually disconnecting a live DJ you can let AzuraCast disable their account for a set amount of time. This task is responsible for re-activating the DJ accounts after the time has passed.

# 1-Minute Sync

<br>

## CheckRequests

This task is responsible for sending user requested songs to the Liquidsoap AutoDJ to play.

## RunBackupTask

This task is responsible for checking if a scheduled backup needs to be run.

## CleanupRelaysTask

This task is responsible for the automatic cleanup of relay data.

# 5-Minute Sync

<br>

## CheckMediaTask

This task is responsible for scanning the media storage location for newly added or modified media files that AzuraCast needs to process.

## CheckFolderPlaylistsTask

This task is responsible for automatically assigning the media of a folder to the playlist that the folder is assigned to.

## CheckUpdatesTask

This task is responsible to check if there are updates available for your AzuraCast installation

# 1-Hour Sync

<br>

## RunAnalyticsTask

This task is responsible for updating daily listener data and analytics that can take time to calculate.

## RunAutomatedAssignmentTask

This task is responsible for running the automatic media assignment to playlists.

## CleanupHistoryTask

This task is responsible for cleaning up old listener and song history data according to the system settings.

## CleanupStorageTask

This task is responsible for clearing the temporary data directory as well as checking the media storage locations for files that need to be removed.

## RotateLogsTask

This task is responsible for rotating logs and backup of the AzuraCast installation.

## UpdateGeoLiteTask

This task is responisble for automatically updating your GeoLite database for IP-address geolocation if you have supplied an API key.