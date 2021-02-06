---
title: Docker
description: 
published: true
date: 2021-02-06T06:43:41.795Z
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

- When updating (using the `docker.sh utility script), it is recommended to run `./docker.sh update-self` before running `./docker.sh update`, to ensure the Docker Utility Script itself is up to date before it updates your Docker installation.

- Environment variables set in `.env are only used by Docker Compose itself, and aren't passed directly into the AzuraCast containers. You should only modify this file to change the HTTP and HTTPS port mappings used by Nginx (see the "Use Non-Standard Ports" section above).

- The `azuracast.env` file is specific to your environment and can be customized however you like. It will not be replaced during any updates. Once your database has been created, however, changing the password listed in this file will cause the system to fail. If you want to destructively wipe your existing database and other files and set up a new one with the updated password, add the `-v` flag to the end of `docker-compose down to remove all existing volumes, including your database.

- If possible, you should not directly modify `docker-compose.yml`, as some updates may modify how it is defined to resolve bugs or add new features. When updating, you will always be asked if you want to update this file; if you have not modified it, you should always do so.

- Instead of modifying `docker-compose.yml`, you can create a file named `docker-compose.override.yml` with your customizations. The structure of this file is the same as the main `docker-compose.yml` file, and is automatically parsed by Docker Compose to override any definitions in the main file. Updates will not replace this file.
