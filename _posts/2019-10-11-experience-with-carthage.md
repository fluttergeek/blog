---
title: Experience With Carthage As Opposed To Cocoapods
date: 2019-10-11T00:00:00.000+00:00
categories:
- Review
- Xcode
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
image: https://koenig-media.raywenderlich.com/uploads/2017/08/Carthage-feature-2.png

---
Cocoapods make your build times slower and that's why I've taken the liberty of trying Carthage. Well, from the instructions I've read in bigger frameworks like `Realm` and `FacebookCore/FacebookLogin`, it is a daunting process. Which is why it took me this long to want to even bother with it. But for some smaller frameworks, it is almost similar to Cocoapod's process, which I like better. Unfortunately, this arduous shift took painfully long to install the frameworks. Every time I add a new framework to my `Cartfile` and update Carthage, it downloads everything and builds everything over again. My gahd!

### Update (Tips from Wolox)

To refrain Carthage from rebuilding, add --cache-builds like so

    carthage update --platform iOS --cache-builds

If your team member is using a version of a dependency, and you want the same version, then use `bootstrap`:

    carthage bootstrap --platform iOS --cache-builds

This will take the last version from the `Cartfile.resolved` and download it instead of whatever the latest version of that dependency is.

If you want to bootstrap an individual dependency/package/framework:

     carthage bootstrap <new dependency> --platform iOS --cache-builds

Apart from that, I didn't know that I had to take this extra step for each of the frameworks I'm using.

![IMPORTING](/blog/assets/images/Screen%20Shot%202019-10-11%20at%2012.38.52%20AM.png)

If I didn't import all of them there, I'd get this error.

![error](/blog/assets/images/Screen%20Shot%202019-10-11%20at%2012.30.16%20AM.png)

There's also another step you have to do:

1. Go to Build Phases
2. Click `+` and add New Run Script Phase
3. Type `/usr/local/bin/carthage copy-frameworks` in the ginormous Textarea under `Shell /bin/sh`

We're not done yet. Phew! For every framework you add, you also need to add their location in the `input files`. The `output files` are optional to put in, but if you complete it, Carthage will be able to do optimizations and take less time to install frameworks on a device. For this, I use `Carting`. It creates a `xcfilelist` for input files and output files for all of it so you don't have to manually encode a location for each of those. [Check it out](https://github.com/artemnovichkov/Carting).

There were a few frameworks that just doesn't work with Carthage, those being:

1. GMStepper - No Carthage available
2. BetterSegmentedControl - Error during Carthage installation
3. Firebase - They will not be maintaining their Carthage archives anymore. They'd rather invest it on `Swift Package Manager`, though I'd still have to look it up one day.

For these three, I maintained the use of Cocoapods. It can works hand in hand with Carthage.

I'm not sure I've noticed any changes, because Firebase is the heaviest framework I have and it is installed with Cocoapods, thereby still making my build time really slow. Apart from that, I don't really see the rest of the frameworks build anymore. I guess that's an improvement.

For more tips on using Carthage, head on to [Wolox's article](https://medium.com/wolox-driving-innovation/how-to-carthage-efficiently-4913b8378898 "Carthage").