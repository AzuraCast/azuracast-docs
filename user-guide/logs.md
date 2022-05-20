---
title: Logs
description: How to get logs and what logs are available
published: true
date: 2022-03-08T22:43:29.586Z
tags: getting started, debugging
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

# Log Types

## AzuraCast Application Log
 
This log file contains the log output of the AzuraCast application itself. If you encounter any errors with the application itself you will most likely find more information about those errors in this log file.

## Liquidsoap Log

This log file contains the log output of the Liquidsoap instance for your station. Liquidsoap is responsible for the audio transcoding in AzuraCast and interfaces with the AzuraCast application to provide the AutoDJ for your station.

In this log you can find information on how the streams are generated, what information the AutoDJ sent to Liquidsoap, errors, warnings and more.

If you encounter any problems with your stream generation this look should contain useful information in diagnosing the issue.

## IceCast Access Log

This log file contains the raw access log data of the IceCast server.

For most issues this log does not contain any useful information but can be used for analytical purposes.

## IceCast Error Log

This log file contains the error output of the IceCast server.

If you encounter any problems with accessing the streams or your mount points this is were should start looking into.

# Docker Container Logs


Some system logs can only be accessed from a shell session on the host computer. You can run `docker-compose logs -f <container_name>` to access container logs from the terminal. Replace `<container_name>` with one of the following container names.

> In order to run this command you need to be in the installation directory of your AzuraCast installation. By default this should be `/var/azuracast/`
{.is-info}

### By Version {.tabset}
#### Stable Release Version (0.16.0) and newer
Container Name | Description
- | - 
`azuracast` | This is the only container we use for AzuraCast, it holds: 
The main AzuraCast PHP application that controls the stations and provides the web
interface, the **MariaDB** Database and the **Redis** server which is used for caching and the old Station container which contains IceCast, ShoutCast and Liquidsoap functionality.
#### Stable Release Version (0.15.0) and older
Container Name | Description
- | - 
`nginx_proxy` | This is the container with the NGINX web server proxy that is responsible for making the AzuraCast application available to browsers. This container is also providing the [Let's Encrypt](/en/administration/ssl-and-lets-encrypt) integration for AzuraCast and Multi-Site installtions. <br><br> **We don't use this container unless you're using Multi-Site**
`nginx_proxy_letsencrypt` | This container is responsible for retrieving and updating the Let's Encrypt certificates for SSL. <br><br> If you encounter any issues with the Let's Encrypt setup you should look into the logs of this container. 
`web` | This container is hosting the main AzuraCast PHP application that controls the stations and provides the web interface. <br><br> If you encounter any server 500 errors or have issues with your sync tasks you should look into the logs of this container.
`mariadb` | This container is providing AzuraCast with the MariaDB database. <br><br> If you encounter any database related issues you should look into the logs of this container.
`redis` | This container is providing the AzuraCast application with a Redis server for caching. This is mainly used for caching the filesystem.
`stations` | This container contains the IceCast, SHOUTcast and Liquidsoap processes which are responsible for generating your streams. <br><br> If you encounter any stream related issues you should look into the logs of this container.

# Ansible Installation Logs

Since the Ansible installation interacts directly with your host server, its logs are in various locations across the system.

- AzuraCast: `/var/azuracast/www_tmp/app.log`
- Nginx Access: `/var/azuracast/www_tmp/access.log`
- Nginx Errors: `/var/azuracast/www_tmp/error.log`
- PHP: `/var/azuracast/www_tmp/php_errors.log`
- Supervisord: `/var/azuracast/www_tmp/supervisord.log`
- Redis: `/var/log/redis/redis-server.log`
- MariaDB: `/var/log/mysql`

For each station, logs for radio software will be inside `/var/azuracast/stations/{station_short_name}/config`, with the following filenames:

Liquidsoap: `liquidsoap.log`
Icecast: `icecast.log`
SHOUTcast: `sc_serv.log`