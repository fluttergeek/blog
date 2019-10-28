---
title: "!= VS. NOT Filter In Realm Swift"
date: 2019-09-30 00:00:00 Z
categories:
- Tutorial
tags:
- ''
- Swift
- Realm
- Filter
- NOT
- Equal
- Inverse
image: assets/images/1_DqzHUZj6HcmIBK9I2aOVww.png
---

Instead of using `!=` as your not equal, you can or should use `NOT` instead. I'm writing this post because I just can't seem `!=` to work as part of my predicate. I was struggling to find a way to make not equal to work. I looked it up in the documentation. Even though `!=` is in there, I could not make it work. However, the `=` sign worked. Weird. And so, I did something like this.

    let filter = "(status = 'Pending' OR (status = 'Washed' AND (pad = 2 OR pad = 3))) AND NOT cancel = 'Cancelled' AND NOT ownerId = ''"
    orders!.filter(filter)

Here, you'll see `NOT` placed before the attribute. I don't know why it works that way, but it works anyway.