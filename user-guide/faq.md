---
title: Frequently Asked Questions (FAQ)
description: 
published: true
date: 2021-02-06T02:31:49.799Z
tags: 
editor: markdown
dateCreated: 2021-02-06T02:31:49.799Z
---

# How do I reset an Account Password?

If you have lost the password to log into an account, but still have access to the SSH terminal for the server, you can execute the following command to generate a new random password for an account in the system.

Replace `YOUREMAILADDRESS` with the e-mail address whose password you intend to reset.

**Docker:**
```
./docker.sh cli azuracast:account:reset-password YOUREMAILADDRESS
```

**Ansible:**
```
php /var/azuracast/www/bin/console azuracast:account:reset-password YOUREMAILADDRESS
```

# Access Files via SFTP

AzuraCast includes a SFTP server built-in on Docker installations, if your machine does not have it [update your system](/en/getting-started/updates) to at least version `0.9.8.1` or higher. The SFTP server uses port 2022 but this can be changed in the `.env` file.

For more information on how to use SFTP, review the [SFTP documentation](/en/user-guide/sftp-server).

#