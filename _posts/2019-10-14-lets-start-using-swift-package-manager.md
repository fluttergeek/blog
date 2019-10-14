---
title: Let's Start Using Swift Package Manager
image: https://miro.medium.com/max/1200/1*ooI0KwILz0Yo2cXUaW9gog.png
tags:
- ''
- tutorial
- swift
- package
- manager
- review
categories: Review
---

The first time I encountered the word `Swift Package Manager` was just a few days ago while searching for alternative ways to install the `Firebase` framework in my project. I thought `Carthage` was a holy grail. Boy, am I wrong now that I've seen how to install frameworks with Swift Package Manager. Unfortunately, Firebase doesn't have a swift package yet. Not officially, anyway. They have started trying to make the support since months ago, but no prorgress has been done months after. Crazy! I know.

The process of adding a framework to your project with SwiftPM is pretty straightforward. Get the git url from the framework's github that is SwiftPM supported. Then in your Xcode, go to `File » Swift Packages » Add Package Dependency`. Enter the git url and click `next`. You will be given an option to choose the latest version or from another branch, typical `Github` jargon really.

That wasn't so hard was it? No more typing on that freaking terminal or creating a `Podfile` or a `Cartfile`. Everything is handed to you by Xcode now. And like, basically, everyone can easily create their own packages and push them to github. I never really had any idea how those pods came to be published or approved after being made by their creators, but this time everyone gets to be creator >:)

And while there's a `cocoapods.org`, there's also [swiftpack.co][spm] to look for existing packages. Alamofire and SwiftyJSON are some of the familiar packages listed in that repository.

There's a ridiculously simple explanation on how to create a `Swift Package`:

<iframe width="560" height="315" src="https://www.youtube.com/embed/xu9oeCAS8aA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[spm]: https://swiftpack.co/
