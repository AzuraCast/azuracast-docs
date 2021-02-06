---
title: Frequently Asked Questions (FAQ)
description: 
published: true
date: 2021-02-06T23:30:56.857Z
tags: 
editor: markdown
dateCreated: 2021-02-06T02:31:49.799Z
---

# Resetting an Account Password

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
