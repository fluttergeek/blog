---
title: Faster Build Time Xcode From HDD
image: assets/images/Pv7si.png
---

I've had my Xcode projects in the HDD drive ever since. Today, I transferred them to my SSD thinking it would solve my problem. But yeah, I guess it would load my codes faster since it's already loading my storyboard faster, but there's a ton of things that Xcode would launch aside from just your code and whatever you've put in your project folder. 

There's this thing called `DerivedData` folder. It is defaultly located in your boot hard drive. It’s the location where Xcode stores all kinds of intermediate build results, generated indexes, etc. DerivedData folder is also infamous for growing up to gargantuan sizes. My latest project alone built up 6.33 GB of derived data after I saw my Xcode index and ran build with this project. Horrible! Because my project folder's contents is only way less than half of that. 2 GB to be exact.

Now that we know where all those extra files are being sent to, the only logical solution is to change the `DerivedData` folder over to your SSD, may it be internal or external. This will in turn make the reads and writes, or writes mostly, faster.

For more tips about optimizing compilation time, read [here][opt].

[opt]: https://codeburst.io/optimizing-compilation-time-for-swift-code-e692376085a6
