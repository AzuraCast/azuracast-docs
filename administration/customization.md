---
title: Customization
description: 
published: true
date: 2021-04-17T12:41:33.766Z
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