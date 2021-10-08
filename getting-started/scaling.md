---
title: Scaling AzuraCast
description: Expectations on what resources are required to host an AzuraCast instance, and tips on how to scale up as your station grows.
published: true
date: 2021-10-08T23:58:32.101Z
tags: 
editor: markdown
dateCreated: 2021-10-08T23:42:49.762Z
---

One of the most common questions we get from our users is "How many stations/listeners/etc. can my server/VPS handle if running AzuraCast?"

Unfortunately, there's no universal answer to this question, as every web host (and indeed, every computer) has slightly different performance characteristics.

From our experiences, we can draw some broad conclusions that can help you in scaling AzuraCast to meet your needs.

## Resource Constraints

### CPU

If you're using the Liquidsoap AutoDJ, there will be a relatively stable but constant CPU load for every single broadcast destination you have on every station.

In this case, a "broadcast destination" is either a local mount point or a remote relay where the main AzuraCast server transcodes the audio and "pushes" it to the remote server. Remote relays that pull the local broadcast and duplicate it don't count toward this limit.

As a very broad guideline, think of the equation for this CPU load as **10% of one CPU per broadcast destination**.

This means if you have one station broadcasting to 10 mount points, you can expect that single station to take up *about* 100% of a single CPU core. Conversely, if you have 10 stations broadcasting only one mount point each, the CPU load is still 100% of a single CPU core.

The CPU impact of listeners connecting to your station is generally *much lower* than the impact of transcoding different stations. The broadcasting frontends (both Icecast and SHOUTcast) can handle hundreds to thousands of listeners before the CPU load of serving those listeners impacts 

#### A Note on Throttling




### RAM

AzuraCast depends on a number of third-party software to operate, and one of the biggest consumers of RAM in our setup is our database engine, MariaDB (a community-maintained fork of MySQL).

Between MariaDB and the main PHP processes, you should allocate at least 512MB, and preferably over 1GB of RAM to any AzuraCast server.


### Storage




## Tips for Overcoming Limits



Your content here