---
title: A Free Blog Using Mundana Jekyll Theme In Github Pages
image: assets/images/Screen%20Shot%202019-09-27%20at%206.00.31%20PM.png
categories: [Jekyll, Tutorial, Admin]
tags: featured
---

This might be too technical for a non computer savvy to be doing, so I've formulated this article to help guide you in creating one of your own. If you want your blog to be in a subdirectory just like my blog `iosjunkie.com/blog`, then you might want to see this first:

<iframe width="560" height="315" src="https://www.youtube.com/embed/nN6QuNqmAwk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It really helped me setup a blog as a subdirectory of my domain. I only had to create another repository `blog` and cloned that repository into my computer. Then I copied Mundana's contents from [Github][mundana] into the blog folder. Next is you'll have to setup the *config.yml*.  If your site is in root, for baseurl, make sure this is set to baseurl: ''. In my case it's baseurl: '/blog'.

The first few lines of config.yml is straightforward. You'll have to change those values. Make sure to change the author attributes as well as the default author under `defaults`. Whenever you post something new, you don't have to indicate a meta for layout, author and avatar anymore. 

![example]({{ site.url }}/blog/assets/images/Screen%20Shot%202019-09-27%20at%206.15.56%20PM.png)
Now, this is what makes life easier. With this dashboard, you don't have to explicity input a meta for your title anymore. Although, you might need to put in the categories, permalink, or tags in your metadata field if you wish to add some.

![metadata]({{ site.url }}/blog/assets/images/Screen%20Shot%202019-09-27%20at%206.29.32%20PM.png)
It's not a perfect system, uploading a photo here can be a pain. Which is why I decided to move a photo asset in the `/assets/images/` directory manually every time, because it saves the photo in my `/blog` folder when I upload using `jekyll-admin`.

It's still a great interface I'm telling you. You can preview your markdown side by side what you've written or by toggling preview only. The texteditor isn't much, but it'll do more than just using an ordinary one.

Install this admin dashboard interface on your Gemfile by adding `jekyll-admin` to it:
```
group :jekyll_plugins do
    gem 'jekyll-feed'
    gem 'jekyll-sitemap'
    gem 'jekyll-paginate'
    gem 'jekyll-seo-tag'
    gem 'jekyll-admin'
end
```

Save the file and go to terminal/CMD. Type `bundle` or `bundle install` to install the gem, but make sure Ruby is installed in your system.

You will only be launching this locally. So, everytime you wan to use jekyll-admin, type this on terminal
```
bundle exec jekyll serve --watch
```
then go to `localhost:4000/admin`

### Posting
I catch myself looking at sample posts whenever I create a new post. You might need to if you're not familiar with the jekyll format or on how to use markdown. So, I renamed the `_post` folder to `post` and added `post` to my .gitignore. It's not necessary to create another `_post` folder back, but if you're not using the jekyll-admin or if you're just using a text editor, then you should create it again.

[mundana]: https://github.com/wowthemesnet/mundana-theme-jekyll.git