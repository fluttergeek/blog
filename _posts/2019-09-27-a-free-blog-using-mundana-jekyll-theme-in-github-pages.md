---
title: A Free Blog Using Mundana Jekyll Theme In Github Pages
date: 2019-09-27 00:00:00 Z
categories:
- Jekyll
- Tutorial
- Admin
tags:
- featured
- mundana
- jekyll
- theme
- gh-pages
- github
- pages
- jekyll-admin
image: assets/images/Screen%20Shot%202019-09-27%20at%206.00.31%20PM.png
---

This might be too technical for a non-computer-savvy to be doing, so I've formulated this article to help guide you in creating one of your own. If you want your blog to be in a subdirectory just like my blog `fluttergeek.com/blog`, then you might want to see this first:

<iframe width="560" height="315" src="https://www.youtube.com/embed/nN6QuNqmAwk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It really helped me setup a blog as a subdirectory of my domain. I only had to create another repository `blog` and cloned that repository into my computer. Then I copied Mundana's contents from [Github](https://github.com/wowthemesnet/mundana-theme-jekyll.git) into the blog folder. Next is you'll have to setup the _config.yml_.  If your site is in the root, for baseurl, make sure this is set to baseurl: ''. In my case, baseurl: '/blog'.

The first few lines of config.yml is straightforward. You'll have to change those values. Make sure to change the author attributes as well as the default author under `defaults`. Whenever you post something new, you don't have to indicate a meta for layout, author, and avatar anymore.

![example](/blog/assets/images/Screen%20Shot%202019-09-27%20at%206.15.56%20PM.png) Now, this is what makes life easier. With this dashboard, you don't have to explicity input a meta for your title anymore. Although, you might need to put in the categories, permalink, or tags in your metadata field if you wish to add some.

![metadata](/blog/assets/images/Screen%20Shot%202019-09-27%20at%206.29.32%20PM.png) It's not a perfect system, uploading a photo here can be a pain. Which is why I decided to move a photo asset in the `/assets/images/` directory manually every time, because it saves the photo in my `/blog` folder when I upload using `jekyll-admin`.

It's still a great interface I'm telling you. You can preview your markdown side by side what you've written or by toggling preview only. The text editor isn't much, but it'll do more than just using an ordinary one.

Install this admin dashboard interface on your Gemfile by adding `jekyll-admin` to it:

    group :jekyll_plugins do
        gem 'jekyll-feed'
        gem 'jekyll-sitemap'
        gem 'jekyll-paginate'
        gem 'jekyll-seo-tag'
        gem 'jekyll-admin'
    end

Save the file and go to the terminal/CMD. Type `bundle` or `bundle install` to install the gem, but make sure Ruby is installed in your system.

You will only be launching this locally. So, every time you want to use jekyll-admin, type this on terminal

    bundle exec jekyll serve --watch

then go to `localhost:4000/admin`

### Posting

I catch myself looking at sample posts whenever I create a new post. You might need to if you're not familiar with the Jekyll format or on how to use markdown. So, I renamed the `_post` folder to `post` and added `post` to my .gitignore. It's not necessary to create another `_post` folder back, but if you're not using the jekyll-admin or if you're just using a text editor, then you should create it again.

If you write codes like I do, then there's a bit of an issue with indentation. Every indentation must be done with four spaces, not with 1 or 2 tabs.

#### Update

I found a solution for the indentation of blocks of codes. I wrote an [article](/blog/jekyll-code-syntax-indentation/) about it.

### Creating Pages

Create your pages in `_pages`, but there's one very important matter to discuss here. You need a permalink meta for every page you make. Otherwise, the URL will become `/_pages/your_new_page`.

![permalink](/blog/assets/images/Screen%20Shot%202019-10-04%20at%2012.51.16%20AM.png)

### Automating

You might want to lessen the technicality of going through the terminal to go to your blog directory and serving Jekyll to access admin. You can do this by opening `Automator`. You have this pre-installed on your Mac OS. Create a new `workflow`, then choose to run shell script like so:

![shell](/blog/assets/images/Screen%20Shot%202019-10-02%20at%209.07.22%20PM.png)

Type in what you'd normally type on your terminal.

    cd /Directory/To/Your/Blog
    source ~/.bash_profile
    bundle exec jekyll serve --watch

If you run it and everything goes smoothly, try opening `localhost:4000/admin` on your browser. That's how you'll know if it is already done launching the server. And when you're done writing this script, you may close the `Automator` and it'll ask you to save the script. Save it as Application to your preferred directory. If you ever need to access the admin dashboard of your blog, just double-click like you would on a normal app. You might be surprised. This also works for pushing all your changes to GitHub.

You can then copy it to your Applications folder and it works just like that. You will see that it is running once there a spinning icon in the menu bar. It will say 0% completed for eternity. You cannot close the server even by canceling the whole process in your menu bar. If you really want to stop the server, go to `Activity Monitor` and force quite `ruby` in one of the processes or memory tab. I haven't figured a workaround on this yet, but it doesn't seem to bother me.

### Other Options

On October 27, 2019, I tried Netlify and wrote a [guide](http://fluttergeek.com/blog/using-netlify-as-a-cms-for-your-jekyll-blog/ "Netlify") about it.

On October 28, 2019, I tried Forestry.io again and wrote my likes and dislikes about it as well as some tips. [Check it out](http://fluttergeek.com/blog/using-forestry-as-a-cms-for-your-jekyll-blog/ "Forestry.io"). I've actually tried Forestry.io once before, but I didn't understand it back then.