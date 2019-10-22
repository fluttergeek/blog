---
image: https://miro.medium.com/max/6000/1*7Zy2OC1nxK-BqmDbGxtPDg.png
title: Sharing UserDefault Between Apps
tags:
- ''
- userdefaults
- swift
- xcode
- apps
- ios
categories: Tutorial Swift
---

I just discovered a way for two apps to communicate with each other with the use of UserDefaults. I thought that UserDefault is only available locally in the app. Sandboxed in its own environment.

The two apps should add `Groups` capability each. 

![group](https://i.stack.imgur.com/R7HLz.png)

The group container string should start with `group`. For example, `group.theCommonContainer`. It's just like a url which apps with the groups capability can access, provided that their container must have `group.theCommonContainer`. But of course, it could be named anything else other than `theCommonContainer`.

## Access

In order to access the UserDefault that is being shared by the apps, access UserDefaults this way:

```
let groupDefaults = UserDefaults(suiteName: "group.theCommonContainer")

// reading something from key
let username = groupDefaults?.string(forKey: "something")
```
