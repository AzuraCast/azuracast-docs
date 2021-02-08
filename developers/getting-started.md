---
title: Getting Started
description: Developing with AzuraCast
published: true
date: 2021-02-08T03:56:48.445Z
tags: development
editor: markdown
dateCreated: 2021-02-06T07:31:58.854Z
---

# Best Practices

Development on the main AzuraCast application should always be applicable to a broad community of radio station operators and not specific features to one station or group of stations.

If you would like to build a set of features specific to one station or group of stations, you should take advantage of AzuraCast's plugin architecture. The plugin system takes advantage of event listeners that are built into AzuraCast itself. Check out the [example plugin](https://github.com/AzuraCast/example-plugin) for more details on what is possible via plugins.

<br>

## Contributing Code Changes

AzuraCast is open-source software, and as part of this dedication to openness and transparency, we fully support contributions from members of the community who are skilled in the languages that we use to build our applications.

A majority of our repositories come with an `.editorconfig` file in the root, which will set many standards for indentation, punctuation and other style items for you automatically. You may need to enable EditorConfig support in your IDE of choice.

If your IDE does not support EditorConfig, the most important standard to remember that we follow is the PHP Framework Interoperability Group's [PSR-12 Extended Coding Style](https://www.php-fig.org/psr/psr-12/) standard.

Accessibility, security, and modern best practices are very important in AzuraCast's development. Any newly contributed code can, and should, take advantage of the full suite of new features made available in `PHP 7.4` and newer.

Contributions are also welcome in the supporting technologies used to make AzuraCast possible, such as:

- Dockerfiles (see [our separate repositories](https://github.com/AzuraCast) for Docker containers)
- [Ansible configuration](https://github.com/AzuraCast/AzuraCast/tree/master/util/ansible) for Ansible installs

# Setting Up a Local Environment

Regardless of your host operating system, it's highly recommended to use **Docker** when developing locally, as it offers portability, convenience, and a very close approximation of how AzuraCast runs on production environments.

For the steps below, we're assuming you've created a folder where you will store all of your AzuraCast-related projects, like `/home/myuser/azuracast/`.

You will need Git and Docker installed locally. If you're on Windows or Mac, the best way to use Docker is via the native [Docker Desktop](https://www.docker.com/products/docker-desktop) applications for those platforms.

For Windows, an installer tool like [Scoop](https://scoop.sh/) is highly recommended for dependencies like Git and your terminal of choice. A third-party shell client like Cmder is also often helpful.

<br>

## Clone the Repositories

Using Git, clone the AzuraCast core repository and the various Docker containers into a single folder. When developing locally, the Docker containers are built from scratch, so you will need those repositories to be alongside the main "AzuraCast" project in the same folder.

> **Note for Windows developers:** Before cloning the repositories, you should ensure your Git is locally configured to not automatically convert line endings from Linux style (LF) to Windows style (CRLF), which will break AzuraCast. You can set this globally by running:
> 
> ```sh
> git config --global core.autocrlf input
> ```

In the same folder, run your platform's equivalent of:

```bash
git clone https://github.com/AzuraCast/AzuraCast.git
git clone https://github.com/AzuraCast/docker-azuracast-nginx-proxy.git
git clone https://github.com/AzuraCast/docker-azuracast-db.git
git clone https://github.com/AzuraCast/docker-azuracast-redis.git
git clone https://github.com/AzuraCast/docker-azuracast-radio.git
```

<br>

## Move into AzuraCast Repository Directory

All commands from this point forward should be run in the `AzuraCast` repository's folder. From the parent folder, run:

```bash
cd AzuraCast
```

to enter the core repository's directory.

<br>

## Copy Default Files

Copy the example files into their proper locations:

```bash
cp sample.env .env
cp azuracast.dev.env azuracast.env
cp docker-compose.sample.yml docker-compose.yml
cp docker-compose.dev.yml docker-compose.override.yml
```

<br>

## Modify the Environment File

AzuraCast can automatically load data "fixtures" which will preconfigure a sample station with sensible defaults, to avoid needing to complete the setup process every time.

To customize how the fixtures load in your environment, open the newly copied `azuracast.env` file and customize the following values:

```
INIT_BASE_URL=docker.local
INIT_INSTANCE_NAME=local development
INIT_ADMIN_EMAIL=
INIT_ADMIN_PASSWORD=
INIT_MUSIC_PATH=/var/azuracast/www/util/fixtures/init_music
```

<br>

## Build the Docker Containers

Build the Docker containers from your local copies by running:

```bash
docker-compose build
```

<br>

## Run the in-container installation

<br>

#### Without Data Fixtures

To run the setup process without preconfiguring the installation in any way, run:

```bash
bash docker.sh install
```

#### With Data Fixtures

To preload sample data (provided in the `azuracast.env` file above) and start with a preconfigured installation, run:

```bash
bash docker.sh install --load-fixtures
```

<br>

## Building Static Assets

AzuraCast uses a special Docker container containing the full static asset build stack. This makes it very easy to rebuild the compiled assets after having made changes to the JS or SCSS files.

To access the static container, run:

```bash
bash docker.sh static npm run build
```

By default, this will clean up the existing asset manifests and build new CSS and JS files. To access the terminal inside this container, run `./docker.sh static bash`.

<br>

## Spin up the Docker containers

By default, AzuraCast will be available at http://localhost/. A self-signed TLS certificate is also provided out of the box, so you can take advantage of the HTTPS functionality after manually exempting the site via your browser.

<br>

## Translations (Locales)

Locales are managed by the application in two places: the backend PHP code and the frontend (primarily Vue.js) JavaScript code. When new locales are added or translations are changed, they should be processed in both locations.

There are two steps to the translation process:

<br>

### Generating New Translations

When new strings have been added to the application, they should be added to the .POT (localization template) file, so that our CrowdIn translation service recognizes it as a translatable string.

This can be done by running the respective "Generate Locales" commands:

```bash
# Backend
bash docker.sh cli locale:generate

# Frontend
bash docker.sh static npm run generate-locales
```

<br>

### Import New Translated Strings

When strings have been translated, they should be converted back into optimized files that can easily be read by the PHP and Vue.js parts of the application, respectively.

This can be done by running the respective "Import Locales" commands:

```bash
# Backend
bash docker.sh cli locale:import

# Frontend
bash docker.sh static npm run import-locales
```