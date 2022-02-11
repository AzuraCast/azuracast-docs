---
title: CloudFlare
description: Using CloudFlare as proxy for your AzuraCast installation
published: true
date: 2022-02-11T18:27:35.112Z
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

## Changes to Port 80/443
Due to CloudFlare's Terms of Service changes, they no longer allow for Port 80 and 443 to be used for non HTML content. Because of the change, we cannot support this any longer as it risks users CloudFlare accounts being permanently terminated for violating the CloudFlare Terms of Service. Please review the entire section of 2.8 below

> **2.8 Limitation on Serving Non-HTML Content**
The Services are offered primarily as a platform to cache and serve web pages and websites. Unless explicitly included as part of a Paid Service purchased by you, you agree to use the Services solely for the purpose of (i) serving web pages as viewed through a web browser or other functionally equivalent applications, including rendering Hypertext Markup Language (HTML) or other functional equivalents, and (ii) serving web APIs subject to the restrictions set forth in this Section 2.8. Use of the Services for serving video or a disproportionate percentage of pictures, audio files, or other non-HTML content is prohibited, unless purchased separately as part of a Paid Service or expressly allowed under our Supplemental Terms for a specific Service. If we determine you have breached this Section 2.8, we may immediately suspend or restrict your use of the Services, or limit End User access to certain of your resources through the Services.
{.is-info}


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

<br>

## Cloudflare Rocket Launcher

Users may run into this following error: 

> `Refused to execute inline script because it violates the following Content Security Policy directive`

In order to resolve this  error, you must go to your CloudFlare Dashboard and disable the Rocket Launcher settings. 
![](https://aws1.discourse-cdn.com/cloudflare/original/3X/5/7/57001bbc0803f75f68d7699b3c76ba83e039cedb.png)

![](https://aws1.discourse-cdn.com/cloudflare/original/3X/f/0/f057f97a3f79811e68d51e6bf86212fb0619659c.png)