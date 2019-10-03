---
title: You may have difficulty booting from this destination volume, the underlying
  disk is not partitioned with a partitioning scheme that Apple recommends for Intel
  Macs.
tags:
- ''
- carbon
- copy
- cloner
- EFI
categories: Tutorial Mac
image: assets/images/Screen%20Shot%202019-10-03%20at%2011.07.54%20PM.png
---

You might encounter this problem after cloning with Carbon Copy Cloner. You've also come here because you're trying to make the cloned hard drive bootable. Well, you're in luck. The problem lies with the partition scheme you've used. If you've set your Disk Utility's view to `Show Only Volumes`, then it would look like this:

![diskutil](/blog/assets/images/Screen%20Shot%202019-10-03%20at%2011.10.19%20PM.png)

You have no option to choose the partitioning scheme of your choice, but if you set the view to `Show All Devices` then you'll see this option like so:

![diskutil](/blog/assets/images/Screen%20Shot%202019-10-03%20at%2011.09.23%20PM.png)

Since you want your disk to be bootable, then set your scheme to `GUID Partition Map`. If you chose Master Boot Record, you'll have no EFI volume hence not bootable. You will then be able to fill your new partition with say a Mac OS or Hackintosh.
