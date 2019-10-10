---
title: Experience With Carthage As Opposed To Cocoapods
image: https://koenig-media.raywenderlich.com/uploads/2017/08/Carthage-feature-2.png
tags:
- ''
- carthage
- swift
- cocoapods
- cocoapod
- pod
- import
- libraries
- framework
- bettersegmentedcontrol
- gmstepper
categories: Review Xcode
---

Cocoapods makes your build times slower and that's why I've taken the liberty of trying Carthage. Well, from the instructions I've read in bigger frameworks like `Realm` and `FacebookCore/FacebookLogin`, it is a daunting process. Which is why it took me this long to want to even bother with it. But for some smaller frameworks, it is almost similar to Cocoapod's process, which I like better. Unfortunately, this arduous shift took painfully long to install the frameworks. Every time I add a new framework to my `Cartfile` and update carthage, it downloads everything and installs everything over again. My gahd! 

Apart from that, I didn't know that I had to take this extra step for each of the framework I'm using. 

![IMPORTING](/blog/assets/images/Screen%20Shot%202019-10-11%20at%2012.38.52%20AM.png)

If I didn't import all of them there, I'd get this error.

![error](/blog/assets/images/Screen%20Shot%202019-10-11%20at%2012.30.16%20AM.png)

There were a few frameworks that just didn't work with Carthage, those being:
1. GMStepper - No carthage available
2. BetterSegmentedControl - Error during Carthage installation
3. Firebase - They will not be maintaining their Carthage archives anymore. They'd rather invest it on `Swift Package Manager`, though I'd still have to look it up one day. 

For those three, I maintained the use of Cocoapods. It can work hand in hand with Carthage.

I'm not sure I've noticed any changes, because Firebase is the heaviest framework I have and it is installed with Cocoapods, thereby still making my build time really slow. Apart from that, I don't really see the rest of the frameworks build anymore. I guess that's an improvement.
