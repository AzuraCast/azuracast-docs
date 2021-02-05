---
title: Logs
description: How to get logs and what logs are available
published: true
date: 2021-02-05T23:38:57.381Z
tags: 
editor: markdown
dateCreated: 2021-02-05T22:33:04.885Z
---

Before submitting any GitHub issues, you should take a look at the terminal logs that AzuraCast outputs. They can often provide additional information about the error, or include very useful information that should be included in any GitHub issue you create.

> Users with the appropriate permissions can also view many logs directly through AzuraCast itself. The Log Viewer feature is available under "Utilities" in each station's management page.
{.is-info}

# System Logs

If you are administrating your own AzuraCast installation you can check most logs through the `Administration` -> `System Logs`.

In addition to the `IceCast`, `SHOUTcast` and `Liquidsoap` logs you can find the `AzuraCast Application Log` here which contains useful information about what the AzuraCast application itself is doing.

# Log Viewer

The `Log Viewer` under `Utilities` in your station gives you access to the Logs from your stations `IceCast`, `SHOUTcast` and `Liquidsoap` processes.

In addition to the logs you can also view the IceCast, SHOUTcast and Liquidsoap configuration scripts that AzuraCast automatically generates for you.

# Docker Container Logs

Some system logs can only be accessed from a shell session on the host computer. You can run `docker-compose logs -f <container_name>` to access container logs from the terminal.

## Available Containers

### nginx_proxy
### nginx_proxy_letsencrypt
### web
### mariadb
### redis
### stations

# Log Types

### AzuraCast Application Log

This log file contains the log output of the AzuraCast application itself. If you encounter any errors with the application itself you will most likely find more information about those errors in this log file.