---
date: 2019-12-24T10:11:17.000+00:00
title: Cannot Delete or Merge Old Volume In Partition In Catalina
categories:
- Mac
- Tutorial
tags:
- partition
- volume
- catalina
- bootcamp
- terminal
- mac
- diskutil
image: ''

---
This post is from StackExchange, coming from a user named [Charles](https://apple.stackexchange.com/questions/235080/cannot-delete-or-merge-old-boot-camp-partition-in-el-capitan/255088#255088 "StackExchange").

> The answer is so simple. Forget terminals and internet recovery. Here's what you gotta do:
>
> 1. Format the BOOTCAMP-partition through disk utility to MS-DOS (FAT)
> 2. Press cmd + space and write bootcamp-assistant, open bootcamp
> 3. Erase windows and merge the partitions as one mac HD-partition.

I'm only sharing this because I've seen so many other ways people have gone through to merge their volumes, and it doesn't look as simple as the way Charles did. The `diskutil` command was daunting and it is more prone to mistakes. You could be typing the wrong volume ID.

Allow me to rephrase the instructions above:

1. Format the volume you want to delete through disk utility to MS-DOS (FAT). You must be here because it can't be deleted because it is the original/first volume in the partition.
2. Press cmd + space to open Boot Camp Assistant app. You will then see what seems like very odd suggestions and has to do with the Windows OS. Just play along and you'll see something like this:

   
   ![](/blog/assets/images/Screen Shot 2019-12-24 at 6.14.59 PM.png)
3. Check the `Install or remove Windows 10 or later version`. Continue.

   
   ![](/blog/assets/images/Screen Shot 2019-12-24 at 6.15.29 PM.png)
4. Check the `Restore disk to a single macOS partition`. Continue. And it is done.

I was confused with the words used by Charles, but then I got to thinking. Windows didn't mean the actual OS here. It is referring to the type of volume that Windows can be installed in, which is MS-DOS (FAT).

To be detected having the Windows in the partition, we needed to change the volume that we want to delete into MS-DOS.