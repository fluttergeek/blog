---
title: Using Netlify As A CMS For Your Jekyll Blog
date: 2019-10-27 16:00:00 Z
categories:
- Jekyll
- Tutorial
tags:
- cms
- netlify
- jekyll
image: assets/images/Screen Shot 2019-10-28 at 11.12.35 AM.png
---

I was actually intimidated by the idea of trying out `Netlify` for my blog but not so much anymore. It turned out pretty easy to do.

First thing's first, `Netlify` is some kind of web hosting. Although you can opt to just run a local server from your computer by just running `bundle exec jekyll serve`.

Let's create our `Netlify` account and follow this guide by Thomas Bradley:

<iframe width="560" height="315" src="https://www.youtube.com/embed/eEjdJY_Ak8U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This is actually a part of his Jekyll + NetlifyCMS hosting setup playlist. I didn't find the rest to be that useful, so I've written down below the next steps you should take.

To create the `Netlify Dashboard`, you need to create a folder. This folder is a subdirectory of your `blog`. So, if you set your baseurl from your `_config.yml` to `\blog`, then this folder, let's say, `admin` can be accessed through `\blog\admin`. This folder has to be created at the root of your Jekyll project folder. Then there are only two files you need to create under the admin folder. `index.html` and `config.yml`. This is another config.yml different from the root's _config.yml.

I copied the contents of `index.html` from [Add to Your Site](https://www.netlifycms.org/docs/add-to-your-site/#app-file-structure "Add to Your Site").

    <!doctype html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Content Manager</title>
    </head>
    <body>
      <!-- Include the script that builds the page and powers Netlify CMS -->
      <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
    </body>
    </html>

Now comes the tricky part, which is setting up the look of our admin dashboard in `config.yml`. If you reverse engineer it, it's easier than reading through html.

There are ways to login to your dashboard, but I chose to login with `Github`.

    backend:
      name: github
      branch: gh-pages # Branch to update (optional; defaults to master)
      repo: iosjunkie/blog # Repository of your blog in Github
    
    publish_mode: editorial_workflow
    media_folder: "assets/images"
    
    collections:
      - name: "posts"
        label: "Posts"
        folder: "_posts"
        create: true
        slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
        fields:
          - { label: "Layout", name: "layout", widget: "hidden", default: "post" }
          - { label: "Title", name: "title", widget: "string" }
          - label: "Featured Image"
            name: "image"
            widget: "image"
            required: false
            media_library:
              config:
                multiple: false
          - { label: "Tags", name: "tags", widget: "list", required: false }
          - { label: "Categories", name: "categories", widget: "string", required: true }
          - { label: "Draft", name: "draft", widget: "boolean", default: true }
          - label: "Body"
            name: "body"
            widget: "markdown"
      - name: "website-settings"
        label: "Settings"
        files:
          - label: "Settings"
            name: "site"
            file: "_config.yml"
            fields:
              - label: "Name"
                name: "name"
                widget: "string"
              - label: "Description"
                name: "description"
                widget: "markdown" 

I just came up with this from the tutorials I watched.

This is what it looks like when I go to `http://localhost:4000/blog/admin/#/collections/website-settings`:

![](blog/assets/images/Screen Shot 2019-10-28 at 11.11.40 AM.png)  
![](blog/assets/images/Screen Shot 2019-10-28 at 11.11.59 AM.png)  
And this is what it looks like in `http://localhost:4000/blog/admin/#/collections/posts:`

![](blog/assets/images/Screen Shot 2019-10-28 at 11.12.13 AM.png)

![](blog/assets/images/Screen Shot 2019-10-28 at 11.12.35 AM.png)