---
title: Troubleshooting
description: Having trouble with AzuraCast? This page has several resources that can help you solve your problem and be back up and running.
published: true
date: 2021-02-08T05:00:16.386Z
tags: getting started, debugging
editor: markdown
dateCreated: 2021-02-05T21:17:05.327Z
---

Our core maintainers, along with our vast community of station operators, can answer many questions you may have about our software. We also have knowledge base articles set up for common issues encountered by our users.

# First Steps

Your first steps should be to check these docs for any tips or pointers. Many common issues can be solved by following the instructions in our guide pages.

If you aren't sure of what is causing your issue, [checking the logs](/en/user-guide/logs) can help. Not only do the system logs often explain your error in more detail, but they're essential for our developers to be able to easily diagnose and resolve your issue.

# Flushing the System Cache

Many parts of the AzuraCast system depend on caches to speed up site performance. Sometimes, these caches can get out of
date, and they may cause errors. You can always flush all site-wide caches using one command-line script:

For Docker Installations:

```bash
./docker.sh cli cache:clear
```

For Ansible Installations:

```bash
php /var/azuracast/www/bin/console cache:clear
```

# Common Docker Errors

<br>

## "bind: address already in use"

If you get this error when starting the Docker AzuraCast instance, it means that something else (some other process or server software) is already running on the same port(s) that AzuraCast is trying to reserve for itself.

There are generally two ways to resolve this:
- Find the process that's currently listening on the port (commands like `netstat -tulpn` can help you with this) and either disable the program that's currently listening, edit its configuration to change the port it listens on, or uninstall it from the server completely.
- Switch the port AzuraCast uses for its own traffic to an unused one. [See instructions](#use-non-standard-ports)

Since this error is caused by the interaction of some piece of software on your host computer and the Docker daemon, we can't automatically resolve it for you, and the best way to resolve it will depend on how your server is configured and what else is running on it.

Common services that listen on ports 80 and 443 include web servers like Apache and nginx. One common port to use if you still need these services to remain online alongside AzuraCast is port 8080, which we deliberately leave open due to its common usage with other software.

<br>

## "WARNING: The LETSENCRYPT_X variable is not set. Defaulting to a blank string."

This message doesn't indicate anything is wrong with your installation; it is simply Docker Compose letting you know that it defaults to an empty string if a certain environment variable isn't present. You will see these messages if you don't have LetsEncrypt configured on your server, and you can safely disregard them.

# Submitting an Issue

If you encounter an issue that you can not resolve with the documentation please refer to our guide on [how to report a bug](/en/contribute/report-a-bug).