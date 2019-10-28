---
title: Using Forestry As A CMS For Your Jekyll Blog
date: 2019-10-27 16:00:00 +0000
categories:
- Jekyll
- Tutorial
tags:
- cms
- netlify
- forestry.io
- forestry
- jekyll
image: assets/images/Screen Shot 2019-10-28 at 2.33.01 PM.png

---
This is by far the best CMS for Jekyll I have discovered. It only takes a few clicks to get it up and running, unlike Jekyll-admin/manager and Netlify. I still haven't found a workaround the error I'm getting with `/.forestry/settings.yml`. It keeps saying `Invalid yaml syntax`.

![](/blog/assets/images/Screen Shot 2019-10-28 at 2.36.31 PM.png)  
However, everything seems to be working just fine with Forestry.

Things I like about it:

1. All the posts can be sorted by either `title` or `date`.
2. I can insert and upload pictures on the markdown text editor with ease. It allows me to choose a photo from the assets folder that I've configured. It also allows me to just input a link instead of choosing from the assets folder.
3. I can also insert and upload an image for my `Featured Image` front matter.
4. The markdown editor looks more professional that `jekyll-manager`.

Things I don't like about it:

1. Whenever you edit an old post, it goes back up to the top of the recent list.
2. Whenever I select an image for my Featured Image in the front matter, it leaves whitespaces on the filename. Rendering `background-image: url(/blog/assets/images/Screen Shot 2019-10-28 at 11.12.35 AM.png);` invalid. A single quote is then required to fix this.
3. You need to switch to `Raw Editor` instead of `WYSIWYG` to embed a YouTube video to prevent the text editor from detecting a link.

## Tips:

* Every time I write a block of code or insert an image in the `markdown`, I have a hard time going to the next line. Merely pressing `Enter` doesn't do the trick. To go to the next line, you must `Shift` + `Enter`.
* You don't have to create a front matter for the body of your article. It took me a while to realize that there's a markdown editor by default. I tried it first on my iPad, that's why it took me a long time to notice the markdown editor, which is more conspicuous when using a web browser on my pc. It's a gigantic view on the right side of my web browser when using the pc, but it looks like this on the iPad.

![](/blog/assets/images/F81E8C91-8257-430E-A75A-562F33452CBF.jpeg)  
![](/blog/assets/images/7CFB1A72-88DA-4872-A596-3FD5804C40A4.jpeg)