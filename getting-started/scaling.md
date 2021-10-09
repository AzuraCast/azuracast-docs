---
title: Scaling AzuraCast
description: Expectations on what resources are required to host an AzuraCast instance, and tips on how to scale up as your station grows.
published: true
date: 2021-10-09T00:08:38.029Z
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

The CPU impact of listeners connecting to your station is generally *much lower* than the impact of transcoding different stations. The broadcasting frontends (both Icecast and SHOUTcast) can handle hundreds or even thousands of listeners before the CPU load of serving those listeners impacts your total CPU in the same way that media transcoding does.

#### A Note on Throttling

Some cloud hosting providers (including our own partners at DigitalOcean) offer "Shared CPU" plans where your instance shares processor time with other instances hosted on the same server.

They are generally able to offer those services affordably by ensuring that none of the instances use near 100% of their CPU load, allowing some extra load to be shared by other customers.

Because media transcoding is a constant CPU load on the server at all hours of the day, there's no way for the hosting provider to share that resource with other customers; as a result, if your load exceeds about 60% on a constant basis, they will often "throttle" your instance, giving it far fewer total CPU resources than before.

If you're running AzuraCast, you will absolutely notice this "throttling" as it will grind your system to a halt and stop most station operations. Sometimes, the only way to recover from this is to completely power down the server and restart it; other times, the throttling is permanent and requires setting up a new server.

There are two long-term solutions to this throttling:
 - Always "overprovision" your server so it never sees a constant CPU load of more than 50% of its total capacity, ensuring it likely won't be throttled, or
 - Invest in the hosting provider's "Dedicated CPU" plans, which, while they are likely to be more expensive, will not encounter this issue at all.

### RAM

AzuraCast depends on a number of third-party software to operate, and one of the biggest consumers of RAM in our setup is our database engine, MariaDB (a community-maintained fork of MySQL).

Between MariaDB and the main PHP processes, you should allocate at least 512MB, and preferably over 1GB of RAM to any AzuraCast server.

In our experience with AzuraCast, the amount of RAM being used does not scale linearly with the number of stations you operate. Almost every process in AzuraCast has a maximum RAM limit that it will reach, after which processes start to take more *time* instead of more *memory*.

### Storage

A typical AzuraCast installation, whether on Docker or Ansible, requires about 2GB of available space to store its own internal software images. You should have at least 5GB of free space available on the server to allow for upgrades to run without any issues.

The amount of storage you need is largely determined by the size of your stations' media libraries. 


## Tips for Overcoming Limits

