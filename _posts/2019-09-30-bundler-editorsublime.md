---
title: Sublime 3 In BUNDLER_EDITOR
date: 2019-09-30 00:00:00 Z
categories:
- Tutorial
- Sublime
tags:
- ''
- sublime
- bundle_editor
- symlink
- editor
- bash
- profile
image: assets/images/SublimeText3.jpg
---

First, let's create a symlink:

```
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
```

Type this on your terminal.

```
open -e ~/.bash_profile
```

Add the following lines and save it.

```
export EDITOR="sublime -w"
export BUNDLER_EDITOR="sublime"
```
