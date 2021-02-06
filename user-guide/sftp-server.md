---
title: SFTP Server
description: The built-in SFTP server for Docker installations
published: true
date: 2021-02-06T01:52:19.099Z
tags: 
editor: markdown
dateCreated: 2021-02-06T01:52:19.098Z
---

> The Built-in SFTP server is only available on the Docker installation.
{.is-info}

All AzuraCast Docker installations come with a built-in SFTP server that allows you to easily connect to your media directory, upload and move files using software of your choice.

# Creating SFTP Users

Under the "Utilities" section of your station, you will see an entry labeled "SFTP Users". Click this to view existing users and the connection details for connecting to your server via an SFTP client of your choice.

# Default Port

By default, the SFTP service listens externally on port 2022. If there is something else using this port or you want to change it for any other reason, you can edit the `.env` file on your host machine and add or modify this line:

```
AZURACAST_SFTP_PORT=2022
```

> Note, this is a file named `.env`, with no filename, and not the `azuracast.env` file
{.is-info}


# SFTP clients
Here's a small list of FTP clients you can use that are known to work with AzuraCast's SFTP system.

- [Filezilla](https://filezilla-project.org/) - Windows, Mac OS X and Linux 32 bit & 64 bit
- [WinSCP](https://winscp.net/eng/download.php) - Windows only
- [Cyberduck](https://cyberduck.io/) - Windows and Mac

A more exhaustive list can be found [here](https://en.wikipedia.org/wiki/Comparison_of_FTP_client_software).