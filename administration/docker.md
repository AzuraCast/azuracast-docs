---
title: Docker
description: 
published: true
date: 2021-02-06T23:16:19.874Z
tags: 
editor: markdown
dateCreated: 2021-02-06T06:41:47.092Z
---

Docker installations come with four files by default:

- `docker.sh`, the Docker Utility Script;

- `.env`, which contains environment variables used by Docker Compose itself;

- `azuracast.env`, which contains customizable environment variables sent to AzuraCast and related services; and

- `docker-compose.yml`, a large file that defines all of the services used by AzuraCast and how they interact.

# Docker Utility Script

If you're using the Docker installation method to run AzuraCast, we have created a helpful utility script to perform common functions without having to type long command names.

<br>

## Download the Utility Script

If you've recently followed the [Docker installation instructions](/en/getting-started/installation/docker), you already have the Docker Utility Script installed. The file is named `docker.sh`.

If you have an older installation, you can use the Docker Utility Script by running these commands inside your AzuraCast directory on your host computer:

```
curl -L https://raw.githubusercontent.com/AzuraCast/AzuraCast/master/docker.sh > docker.sh
chmod a+x docker.sh
```

<br>

## Run Command Line Tools

```
./docker.sh cli [command_name]
```

Runs any command exposed by the command line interface tools.

<br>

## Install AzuraCast

```
./docker.sh install
```

Pulls the latest version of all Docker images and sets up the AzuraCast database. When complete, your AzuraCast instance should be up and running.

<br>

## Update AzuraCast

```
./docker.sh update-self
./docker.sh update
```

Note: You should always run update-self first to update the updater script.

Automatically pulls down any updated Docker images and applies any database and configuration changes since your last AzuraCast update.

<br>

## Uninstall AzuraCast

```
./docker.sh uninstall
```

Turns off and permanently deletes both the AzuraCast Docker containers and permanent volumes that store the AzuraCast database and station media.

> This command will fully remove any station media, statistics and metrics, and the entire database associated with your AzuraCast instance.
{.is-warning}

<br>

## Back Up Files and Settings

```
./docker.sh backup [/path/to/backup.tar.gz]
```

Creates a .tar.gz backup copy of the media, statistics and metrics of every station, along with a copy of the full AzuraCast database. You can later restore from this same file in the event of data loss or corruption.

<br>

## Restore Files and Settings

```
./docker.sh restore /path/to/backup.tar.gz
```

Extracts a .tar.gz file previously created by this same script's backup command, copying media, statistics and metrics for each station into AzuraCast and importing the version of the database contained in the backup.

> Restoring from a backup will remove any existing AzuraCast database or media that exists inside the Docker volumes.
{.is-warning}

<br>

## Set Up LetsEncrypt

```
./docker.sh letsencrypt-create mydomain.example.com
```

If you want your AzuraCast installation to support HTTPS, one of the easiest ways of accomplishing this is with Let's Encrypt, a free provider of SSL certificates.

Once you have a domain name pointed to your AzuraCast installation, you can run the command above, specify your domain name, and AzuraCast will automatically verify your domain name and update the server with the SSL certificate.

> Your LetsEncrypt certificate is valid for 3 months. The web service will automatically attempt to renew certificates every night.
{.is-info}

# Customizing Docker Installations

For power users looking to customize or expand their Docker configuration, you should follow these best practices:

- Do not modify or replace the `docker.sh utility script.

- When updating (using the `docker.sh` utility script), it is recommended to run `./docker.sh update-self` before running `./docker.sh update`, to ensure the Docker Utility Script itself is up to date before it updates your Docker installation.

- Environment variables set in `.env` are only used by Docker Compose itself, and aren't passed directly into the AzuraCast containers. You should only modify this file to change the HTTP and HTTPS port mappings used by Nginx (see the "Use Non-Standard Ports" section above).

- The `azuracast.env` file is specific to your environment and can be customized however you like. It will not be replaced during any updates. Once your database has been created, however, changing the password listed in this file will cause the system to fail. If you want to destructively wipe your existing database and other files and set up a new one with the updated password, add the `-v` flag to the end of `docker-compose down to remove all existing volumes, including your database.

- If possible, you should not directly modify `docker-compose.yml`, as some updates may modify how it is defined to resolve bugs or add new features. When updating, you will always be asked if you want to update this file; if you have not modified it, you should always do so.

- Instead of modifying `docker-compose.yml`, you can create a file named `docker-compose.override.yml` with your customizations. The structure of this file is the same as the main `docker-compose.yml` file, and is automatically parsed by Docker Compose to override any definitions in the main file. Updates will not replace this file.

# Modifying Docker

The AzuraCast Docker installation is built to serve the needs of the vast majority of installations out of the box, but there may be times when you need to customize how Docker serves your installation, while still retaining the benefits of compatibility and easy updating that Docker offers.

<br>

## The Overall Infrastructure

We have created a simple diagram to explain how our Docker infrastructure is separated into individual containers that handle each major component of the installation:

![docker_infrastructure.png](/images/docker/docker_infrastructure.png)

> InfluxDB is not used for time series and analytics data in AzuraCast anymore. This task is handled with MariaDB now.
{.is-info}


In summary, AzuraCast has five major containers that handle the application's functionality:

- `web`, which contains the main web server (nginx), the application language (PHP), and the built-in SFTP service,

- `stations`, which contains the broadcasting backend (Liquidsoap) and frontend (Icecast/SHOUTcast) for every station,

- `mariadb` which contains the application database running on the MariaDB database engine,

- `redis` which is a high-performance cache used for sessions and other cacheable data, and

<br>

## Docker Compose

In order to make our Docker installation simple to maintain, we rely on a secondary piece of software called [Docker Compose](https://docs.docker.com/compose/). Rather than using lengthy command-line commands, Docker Compose allows you to write simple YAML files to configure the way our Docker containers launch, which ports they forward to the host, mounted disk volumes, etc.

If you follow our installation instructions (or use one of our prebuilt images), your Docker installation is located on your host computer at `/var/azuracast`. Within this folder, you'll see four files by default:

- `docker-compose.yml`, the primary Docker Compose configuration file. AzuraCast's updater automatically prompts you to keep this file updated, which is strongly recommended;

- `docker.sh`, the Docker Utility Script that provides easy aliases for common Docker tasks, such as updating, executing commands inside containers, and more;

- `azuracast.env`, a list of environment (configuration) variables that are sent to the AzuraCast application itself, running inside the web container; and

- `.env`, a separate environment file that affects how Docker Compose _itself_ is configured, and is thus used for higher-level configuration changes like port mappings.

<br>

## Overriding Docker Compose

Rather than modifying `docker-compose.yml` directly, it is strongly recommended to instead create a new file called `docker-compose.override.yml` in the same folder. Docker Compose will automatically look for and include this file if it exists, and apply its configuration on top of the base file's instructions.

There are some considerations when creating your own `docker-compose.override.yml`:

- The file must be valid YAML using the standard [Docker Compose format](https://docs.docker.com/compose/compose-file/compose-file-v2/).

- The file must use the same "version" header as the main `docker-compose.yml` file, currently `2.2`.

- When modifying ports or other lists, you can only add new items, not remove existing ones.

Once you've modified your Docker Compose configuration, you should apply these changes by running:

```sh
docker-compose down
docker-compose up -d
```

Note that this will temporarily shut down your AzuraCast installation and will briefly disconnect your listeners.