---
title: Optimizing AzuraCast
description: Tips for how to get the most out of AzuraCast on limited-resource installations.
published: 1
date: 2023-04-25T05:20:49.003Z
tags: 
editor: markdown
dateCreated: 2023-04-25T05:09:29.560Z
---

If you're running AzuraCast on limited system resources, you may find that you need to tweak or remove some optional features to improve your overall performance. This page includes some of the simplest changes you can take to reduce your overall resource (CPU, RAM, etc.) usage.

## Inside AzuraCast

### Disable Meilisearch

Meilisearch is a built-in tool that makes media manager and requestable song searches much faster, but to do this, it can often take up a large amount of memory. For memory-constrained systems, it's best to disable this, as the search functionality will still work without it (just a little slower).

In `azuracast.env` on the host, update or add the following line:

```
ENABLE_MEILISEARCH=false
```

You can then restart your system (i.e. with `./docker.sh restart`) to disable Meilisearch.

<br>

### Disable Audio Post-Processing

If your installation is limited in available CPU, you should disable any "Audio Post-Processing" settings on any stations hosted on your server.

The total amount of extra resources per station consumed by post-processing is roughly as so:

 - Stereo Tool (Most CPU)
 - `master_me` (Med-High CPU)
 - Basic Compression/Normalization (Low-Med CPU)
 - No Post-Processing (Least CPU)

<br>

### Disable Unnecessary Mount Points

If you have configured multiple mount points for your station, consider reducing the total number of mount points to a smaller subset if possible. Every mount point that the AutoDJ has to broadcast to uses a small, but steady, amount of CPU at all times.

For example, if you offer a mobile MP3 and AAC/Ogg stream, consider limiting the mobile stream strictly to the (higher-performance per bit) AAC/Ogg stream, if your clients support it.

<br>

### Keep Less Playback History

In "System Settings", you will notice a setting called "Days of Playback History to Keep". This setting not only controls how far back you can view song history, but also how far back you can view detailed listener statistics.

Setting it to a large value or "Indefinitely" will quickly fill up your available hard drive space. If you change this setting, within an hour or so AzuraCast will automatically "trim" any extra song history and listener data, which will often result in a significant reduction in disk space used.

<br>

### Use Remote Storage for Media, Backups, etc.

If your VPS or cloud hosting environment is limited in disk space, you can always configure storage locations that use external storage instead.

The most popular method of remote storage in AzuraCast is S3-compatible bucket storage. There are very many providers of S3-compatible storage out there, from the namesake Amazon's Simple Storage Service (S3) to Cloudflare's R2, Backblaze B1, or Wasabi. Pricing for storage and bandwidth can vary greatly between providers, so consider which option is best for your setup.

If configuring a remote storage location for station media, be sure to select one that has little or no so-called "egress" bandwidth to your server, as there is a lot of back-and-forth traffic between AzuraCast and media storage locations. This is less of a concern if you are using S3 buckets for backups or recordings.

## Docker

### Disable Userland Proxy

By default, Docker ships with a "Userland Proxy" feature, which allows you to connect to your Docker containers from inside the same Linux OS that's running Docker itself. For most VPS or cloud hosting scenarios, this is unnecessary, and the userland proxy can often take up *huge* amounts of resources because of the large number of ports forwarded by AzuraCast.

Unfortunately, we can't automatically disable this functionality, but you can configure Docker on your host OS to disable it.

On your host OS, use an editor to open `/etc/docker/daemon.json`. Add or edit the following line into the JSON:

```json
{
  "userland-proxy": false,
}
```

Save your changes, then restart the host OS.