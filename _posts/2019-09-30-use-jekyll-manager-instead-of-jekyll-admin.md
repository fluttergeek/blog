---
title: Use Jekyll Manager Instead of Jekyll Admin
date: 2019-09-30 00:00:00 Z
categories:
- Jekyll
- Admin
- Manager
tags:
- ''
- featured
- jekyll-manager
- jekyll-admin
- blog
- dashboard
- admin
- grammarly
image: assets/images/Screen%20Shot%202019-09-29%20at%2011.31.40%20PM.png
---

I was hesitant in making a change at first, since I just started blogging using Jekyll. Looking at how updates are freshly being added to the jekyll-admin library made me think it's more intuitive. I am wrong. However, the upgrades done in jekyll-manager isn't really all that drastic. They still haven't got a fix for when selecting the image metadata. It is still http://localhost:4000/blog/assets/images/...jpg when I select an image from the static files. I wonder why they haven't dealt this yet. This should have been the turning point of their fork. Instead, a few UI changes have been made. Some, impressive.

![assets](/blog/assets/images/Screen%20Shot%202019-09-29%20at%2011.32.59%20PM.png)

This is my favorite part about jekyll-manager and to be honest, the only thing I care about, really. Directories can be navigated to now, therefore making uploading a hell of a lot easier, now that I'm confident to use this dashboard for uploading images. I no longer have to drag and drop using Finder for this. 

The metadata too has changed. It is, by default, hidden and expandable. It's quite confusing why it's called a `FRONT MATTER`, instead of `metadata`. And by default too, `layout` is the first metadata you'll be able to see. There's a select dropdown for the choices for `layout`. Since in our config.yml, layout is set to `post` by default, then you can choose to remove this metadata field. `tags` has this cool feature that allows you to select from previous used tags. However, this feature is still buggy. I found a bug when I try removing a word. It removes the previous word instead of the current word (tag) that needs removing. It's just sad that it doesn't work the same with `categories`. Hence, when typing multiple categories, just separate them with spaces and not a comma.

The text editor's panel for tools is now dark but overall, it works the same and still no Grammarly functionality. I mean `Grammarly` is disabled when using the text editor, but not on other text fields.  Bummer.

For the most part, it looks almost exactly like jekyll-admin and better, but I don't understand why there isn't any noticeable changes in jekyll-admin that will make it better than the 2 years ago maintaned code of jekyll-manager.

### Automating
You might want to lessen the technicality of going through terminal to go to your blog directory and serving jekyll to access admin. You can do this by opening `Automator`. You have this preinstalled on your Mac OS. Create a new `workflow`, then choose to run shell script like so:

![shell](/blog/assets/images/Screen%20Shot%202019-10-02%20at%209.07.22%20PM.png)

Type in what you'd normally type on your terminal.

```
cd /Directory/To/Your/Blog
source ~/.bash_profile
bundle exec jekyll serve --watch
```

If you run it and everything goes smoothly, try opening `localhost:4000/admin` on your browser. That's how you'll know if it works. And if it does, you may close the `Automator` and it'll ask you to save the script. Save it as Application to your preferred directory. If you ever need to access the admin dashboard of your blog, just double-click like you would on a normal app. You might be surprised. This also works for pushing all your changes to github.
