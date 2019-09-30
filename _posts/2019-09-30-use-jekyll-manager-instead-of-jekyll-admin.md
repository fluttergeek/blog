---
title: Use Jekyll Manager Instead of Jekyll Admin
image: assets/images/Screen%20Shot%202019-09-29%20at%2011.31.40%20PM.png
categories: Jekyll Admin Manager
tags:
- ''
- featured
- jekyll-manager
- jekyll-admin
- blog
- dashboard
- admin
- grammarly
---

I was hesitant in making a change at first, since I just started blogging using Jekyll. Looking at how updates are freshly being added to the jekyll-admin library made me think it's more intuitive. I am wrong. However, the upgrades done in jekyll-manager isn't really all that drastic. They still haven't got a fix for when selecting the image metadata. It is still http://localhost:4000/blog/assets/images/...jpg when I select an image from the static files. I wonder why they haven't dealt this yet. This should have been the turning point of their fork. Instead, a few UI changes have been made. Some, impressive.

![assets](/blog/assets/images/Screen%20Shot%202019-09-29%20at%2011.32.59%20PM.png)

This is my favorite part about jekyll-manager and to be honest, the only thing I care about, really. Directories can be navigated to now, therefore making uploading a hell of a lot easier, now that I'm confident to use this dashboard for uploading images. I no longer have to drag and drop using Finder for this. 

The metadata too has changed. It is, by default, hidden and expandable. It's quite confusing why it's called a `FRONT MATTER`, instead of `metadata`. And by default too, `layout` is the first metadata you'll be able to see. There's a select dropdown for the choices for `layout`. Since in our config.yml, layout is set to `post` by default, then you can choose to remove this metadata field. `tags` has this cool feature that allows you to select from previous used tags. It's just sad that it doesn't work the same with `categories`. Hence, when typing multiple categories, just separate them with spaces and not a comma.

The text editor's panel for tools is now dark but overall, it works the same and still no Grammarly functionality. I mean `Grammarly` is disabled when using the text editor, but not on other text fields.  Bummer.

For the most part, it looks almost exactly like jekyll-admin and better, but I don't understand why there isn't any noticeable changes in jekyll-admin that will make it better than the 2 years ago maintaned code of jekyll-manager.
