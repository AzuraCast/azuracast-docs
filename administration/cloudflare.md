---
title: CloudFlare
description: Using CloudFlare as proxy for your AzuraCast installation
published: true
date: 2021-02-09T03:31:39.980Z
tags: administration
editor: markdown
dateCreated: 2021-02-06T07:13:38.279Z
---

[CloudFlare](https://cloudflare.com/) is a leading provider of reverse proxying and CDN services for the web. Their free tier offers huge benefits in performance and protection that you can take advantage of while using AzuraCast, with a few important caveats. This document details how to use CloudFlare with AzuraCast.

# Enabling CloudFlare with AzuraCast

AzuraCast has full support for using CloudFlare's protection in front of your radio server.

Enabling CloudFlare support from the CloudFlare control panel is as simple as ensuring the little "cloud" next to your radio server's domain (or subdomain) is orange, indicating protection is enabled.

![cloudflare_enable.png](/images/cloudflare/cloudflare_enable.png)

The default settings for CloudFlare will suffice, though you can also switch from "Flexible" to "Full" SSL mode if you like (since AzuraCast itself supports HTTPS). "Strict" SSL mode isn't recommended, as this requires that you maintain an up-to-date SSL certificate on AzuraCast itself (which is possible, but not necessary).

Once you've enabled CloudFlare support for your domain (or radio station's subdomain) from the CloudFlare control panel, you only need to enable one feature inside AzuraCast to enable listeners to connect:

# Enabling the Web Proxy for Radio Broadcasts
One major limitation imposed by CloudFlare is that they do not forward incoming connections to your server that don't come from the traditional web ports (that is, 80 and 443). By default, AzuraCast serves each radio station on its own distinct port in a range from 8000 to 9000. This means your listeners wouldn't normally be able to connect.

![cloudflare_proxy.png](/images/cloudflare/cloudflare_proxy.png)

Fortunately, we've already built a solution to this problem! In AzuraCast's system administration, on the "System Settings" page, we have a checkbox labeled "Use Web Proxy for Radio". Enable this checkbox and all of the station playback URLs across the system will be updated to automatically use the web port proxy links, which are fully accessible even when CloudFlare protection is enabled.

# Important Notes

<br>

## Incompatibility with Rocket Loader
Due to the strict Content Security Policy in AzuraCast you will have to disable CloudFlare's Rocket Loader or your AzuraCast interface will break.

If you have enabled CloudFlare for your AzuraCast installation and your Dashboard shows up like this you should check if your browser's JavaScript console shows the following error and disable Rocket Loader.

![cloudflare_rocket_loader_issue.png](/images/cloudflare/cloudflare_rocket_loader_issue.png)

```
Refused to load the script 'https://ajax.cloudflare.com/cdn-cgi/scripts/a2bd7673/cloudflare-static/rocket-loader.min.js' because it violates the following Content Security Policy directive: "script-src 'self' 'unsafe-eval' 'nonce-oRJ+doQCy3ixkj24VkvoznVv'". Note that 'script-src-elem' was not explicitly set, so 'script-src' is used as a fallback.
```

<br>

## Bandwidth Savings
The nature of streaming radio means it's all but impossible for a service like CloudFlare to cache the radio signal itself using its servers; instead, it passes all of this traffic through to your server directly. This means that CloudFlare will not help you avoid hitting hosting provider bandwidth quotas for your radio service itself.

However, this doesn't mean that you won't see any bandwidth reduction by using CloudFlare; if you use the AzuraCast public pages or its "Now Playing" API, CloudFlare caches both very well and offers significant reductions in the bandwidth needed to serve those pages.

<br>

## Incoming DJ Connections

Because CloudFlare blocks any incoming connections that aren't on the standard web ports, it also blocks the incoming connections that your streamers/DJs would use to broadcast to your station. Unlike the experience we offer listeners through our radio proxy, we can't proxy the incoming broadcast in the same way for technical reasons.

We recommend instructing your streamers/DJs to connect to your server using its IP address rather than its CloudFlare-protected domain name. This will allow them to connect without any issue, and without exposing your origin IP to your regular listener base.

<br>

## AzuraRelay Instances

If you use AzuraRelay instances that should relay a CloudFlare-protected installation, you should use the IP address of the installation as the base URL for the relay (in a format like http://127.0.0.1), rather than the public-facing CloudFlare-protected address. Otherwise, no changes are needed.