---
title: Dropbox Storage Location Configuration
description: 
published: true
date: 2021-02-22T22:50:13.200Z
tags: 
editor: markdown
dateCreated: 2021-02-22T22:50:10.364Z
---

In order to create a Dropbox Storage Location in AzuraCast you will first need to create an OAuth 2 access token for your Dropbox account.

Go to the [Dropbox App Console](https://www.dropbox.com/developers/apps) and click on the `Create app` button.

Select the following options:

- `1. Choose an API`
  - `Scoped access`
- `2. Choose the type of access you need`
  - If you are using your Dropbox other things apart from AzuraCast use `App folder` to automatically create a separate folder in your Dropbox account for this app
  - Otherwise select `Full Dropbox`
- `3. Name your app`
  - Give your app a name, it is recommended to choose a name specific to your radio
  
Then click on `Create app` to finish this step.

On the now generated page for you app switch to the `Permissions` tab at the top of the page and select the following permissions:

- `files.metadata.write`
- `files.metadata.read`
- `files.content.write`
- `files.content.read`

Then click on the `Submit` button in at the bottom of the screen.

Now switch back to the `Settings` tab at the top of the page.

Change the `Access token expiration` from `Short-lived` to `No expiration` and under `Generated access token` click on `Generate`.

Copy the token that is now shown to you and paste it into the `Dropbox Auth Token` text box when creating a new Storage Location of type `Remote: Dropbox` in AzuraCast.