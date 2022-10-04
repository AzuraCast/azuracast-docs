---
title: Customization
description: 
published: true
date: 2021-12-26T23:21:07.060Z
tags: administration
editor: markdown
dateCreated: 2021-02-06T06:22:44.422Z
---

# Customizing the CSS

AzuraCast provides you with the possibility to customize the default CSS via the `Custom Branding` page at `/admin/branding`.

There you can alter the styling of the internal and public pages via your own CSS with the editors for `Custom CSS for Public Pages` and `Custom CSS for Internal Pages`.

[This page](/en/administration/customization/css) will give you a simple overview of the CSS classes and ids used in the HTML of AzuraCast that you can then use in your own CSS to customize those elements.

If you have no prior experience with CSS take a look at the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS) about CSS.

# Custom JavaScript for Public Pages

AzuraCast provides you with the possibility to add custom JS via the `Custom Branding` page at `/admin/branding`.

There you can add your own JavaScripts to the public pages with the editor for `Custom JS for Public Pages`.

You can attach to events of the public player via the global `Vue_PublicFullPlayer` variable like this:

```
Vue_PublicFullPlayer.$eventHub.$on('np_updated', function(np_new) {
    // custom code with np_new
});
```

# Video Playback for Public Page
You can use `Custom JS for Public Pages` to add a video element to the page and style via the `Custom CSS for Public Pages` using our example below. 

**Custom CSS for Public Pages**
```CSS
[data-theme] body.page-minimal .background-video {
	width: 100vw;
	height: 100vh;
	object-fit: cover;
	position: fixed;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	z-index: -1;
}
```
**Custom JS for Public Pages**
``` JS
let videoBackgroundElement = document.createElement('video');
videoBackgroundElement.autoplay = true;
videoBackgroundElement.loop = true;
videoBackgroundElement.muted = true;
videoBackgroundElement.poster = 'Enter.URL.Here';
videoBackgroundElement.className = 'background-video';

let videoBackgroundSource = document.createElement('source');
videoBackgroundSource.src = 'Enter.URL.Here';
videoBackgroundSource.type = 'video/mp4';

videoBackgroundElement.appendChild(videoBackgroundSource);

document.body.append(videoBackgroundElement);
```