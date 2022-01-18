---
title: Multi-Site Docker Installation
description: 
published: true
date: 2022-01-18T08:47:53.598Z
tags: advanced feature, administration, docker
editor: markdown
dateCreated: 2021-02-06T23:21:29.359Z
---

# AzuraCast Multi-Site Docker Installation

If you use the recommended Docker installation method for AzuraCast, you can configure your installation to sit behind a nginx proxy that can also handle other web sites on the same host. This allows you to host, for example, your station's web site on the same server as your AzuraCast installation.

Follow the steps below to configure your installation for multisite support.

## Enter AzuraCast Base Directory

Connect to your server via SSH and change to the base directory where your AzuraCast installation is located. If you followed the recommended installation instructions (or used a prebuilt image) you can run:

```bash
cd /var/azuracast
```

<br>

## Turn Off Existing Services

While you're making these configuration changes, your Docker containers should be turned off. Turn off your existing Docker containers by running:

```bash
docker-compose down
```

This will disconnect your listeners and stop your station from broadcasting. You will not lose any data or statistics during the upgrade.

<br>

## Edit AzuraCast Web Serving Ports

The first step is to prevent AzuraCast's own `web` container from trying to listen on ports 80 and 443, as a proxy will now be doing that job instead.

There is a file in the AzuraCast installation directory named `.env` which has two values set by default:

```
AZURACAST_HTTP_PORT=80
AZURACAST_HTTPS_PORT=443
```

**Edit the `.env` file** in your editor of choice to change those values to an unused public-facing port. A good option would be the examples below:

```
AZURACAST_HTTP_PORT=10080
AZURACAST_HTTPS_PORT=10443
```

Save your changes and return to the shell.

<br>

## Download Custom Override File

We have set up a custom override file that configures both the nginx proxy and its own automated LetsEncrypt support (which is slightly different from our own).

You can download our custom override file by running the command below:

```bash
cp docker-compose.override.yml docker-compose.override.bak.yml
curl -fsSL https://raw.githubusercontent.com/AzuraCast/AzuraCast/master/docker-compose.multisite.yml > docker-compose.override.yml
```

Note that if you've already set up an override file with other modifications, you should make sure to apply the same changes that are now located in `docker-compose.override.bak.yml` to the newly downloaded `docker-compose.override.yml` file.

<br>

## Optional: Configure LetsEncrypt

You can enable LetsEncrypt on the multisite setup the same way as normal LetsEncrypt setup:

```bash
./docker.sh letsencrypt-create
```

If these values are configured and the domain name you specify resolves to your server, LetsEncrypt will automatically set up your SSL certificates and keep them renewed behind the scenes.

<br>

## Start Docker Services

Turn the Docker AzuraCast containers back on to resume broadcasting with the new setup:

```bash
docker-compose up -d
```

## Adding Other Containers

Once you've set up the nginx proxy container, any other Docker container that is spun up on the server and has the `VIRTUAL_HOST` environment variable set will be served by the same proxy service.

For more information, you can visit the [nginx-proxy repository documentation](https://github.com/jwilder/nginx-proxy).

<br>

### Example: Wordpress Site

A common scenario our users encounter is wanting to host their station's homepage on the same server as their AzuraCast instance. This example will walk you through one way of accomplishing this using the official Wordpress Docker image.

<br>

#### Create New Directory

You should begin by creating a new directory that's separate from the main AzuraCast directory. In our example, we're using `/var/wordpress` for ease of explanation.

```bash
mkdir -p /var/wordpress
cd /var/wordpress
```

<br>

#### Create `docker-compose.yml` File

Inside the new directory, create a new file named `docker-compose.yml` based on the format below. Note the areas highlighted with comments, which indicate things you should change:

```yml
services:
    wordpress:
        image: wordpress:latest
        depends_on:
         - db
        networks:
         - azuracast_frontend
         - backend
        restart: always
        environment:
            # Change this to the domain your Wordpress site should be served on.
            VIRTUAL_HOST: wordpress.example.com
            # If you want to use LetsEncrypt on this domain, uncomment these and update them.
            # LETSENCRYPT_HOST: wordpress.example.com
            # LETSENCRYPT_EMAIL: youremail@example.com
            WORDPRESS_DB_HOST: db:3306
            # If you customize these values, make sure to customize them below also.
            WORDPRESS_DB_USER: wordpress
            WORDPRESS_DB_PASSWORD: wordpress
            WORDPRESS_DB_NAME: wordpress

    db:
        image: mysql:5.7
        volumes:
         - db_data:/var/lib/mysql
        networks:
         - backend
        restart: always
        environment:
            # If you customize these values, make sure to customize them above also.
            MYSQL_ROOT_PASSWORD: somewordpress
            MYSQL_DATABASE: wordpress
            MYSQL_USER: wordpress
            MYSQL_PASSWORD: wordpress

networks:
    azuracast_frontend:
        external: true
    backend:
        driver: bridge

volumes:
    db_data: {}
```
   
Once created and saved, you can spin up the new service by running `docker-compose up -d` in the same directory.
