---
date: 2019-11-07T05:23:44.000+00:00
title: From Swift UIKit to Dart Flutter Review
categories:
- Review
tags:
- experience
- review
- flutter
- swiftui
- dart
- uikit
- swift
image: assets/images/flutter_banner.jpg

---
I've given this some thought for maybe a month or two now. To switch from iOS developer to Flutter developer, even though I'm not employed or something. Now, why would I do that? Xcode is the best IDE out there, why switch to a newbie framework that only has VSCode as an IDE?

Well, since I'm unemployed that means I'm a bit closer to the broke spectrum. I was thinking about what if Apple keeps upgrading its hardware requirements and I'm no longer able to keep up with it. I realized that when I had to run a Catalina on this Hackintosh device I'm using. It didn't go so well. I couldn't use SwiftUI's canvass feature. I assume that I'm never going to be able to try it anymore. So what's next if they upgraded even further? And even with UIKit, as my project gets bigger and bigger, the lag on Xcode becomes more and more unbearable.

So I thought, I'd have to just deal with a lightweight IDE to get by. Since I still want to develop on a mobile platform, Flutter has to be the best thing out there. Its community is growing fast, and therefore it is easier for newbies like me to find solutions.

I have tried coding in SwiftUI a bit, but never built a fully functional app with it. I guess I can compare learning Flutter and SwiftUI since they are both declarative. Both can get painfully spaghetti as the project grows. It's totally the least I like about using either. If you've been a web developer, it's also comparable to that. Flutter in dart is like having HTML, javascript, and CSS all combined to become a programming language that is object-oriented. So you see, the pain in having to code every bit of structure is there. It's not the drag and drop that I see in Xcode anymore or the utility area which makes my code a lot slimmer.

Flutter feels like coding a javascript framework, but you can do it in one file if you feel like it. That file would be main.dart. It should always be under the `/lib` folder and not in the subfolders that you might make. The Gemfile I used to work with is now called pubspec.yaml. Once edited, it updates the packages automatically without having to run a command on your terminal. I guess it works that way because of the extensions I installed in VSCode that makes Flutter a lot less tedious to work with. Here's the list of extensions that I have installed:

 1. Auto-save on Window Change
 2. Awesome Flutter Snippets
 3. Beautify
 4. Better Align
 5. Better Comments
 6. bloc
 7. Bookmarks
 8. Bracket Pair Colorizer 2
 9. Color Highlight
10. Dart
11. Flutter
12. Guides
13. Image Preview
14. Material Icon Theme
15. Project Manager
16. Pubspec Assist
17. TabOut
18. Todo Tree

Some of these might not even be specifically for Flutter. It's also good to know that these extensions are easily found on like some kind of AppStore inside the VSCode.

## Learning Curve

A week ago, I could hardly make an idea come to life with Flutter because it looks very limiting to use. I attribute it mostly to my poor knowledge-base of the kinds of widgets there are. The first things I had to understand were Stateful and Stateless. I'm not going to dive deeply into it but I'll try to explain it briefly.

**Stateless** are widgets that will have its contents reloaded, like all of it in that widget. If you want to change any data on this type of widget, it would be like creating an object again in place of the previous one because widgets are objects made up of a `class`.

**Stateful** is particularly going to reload only the data and not the entire build of that widget. This involves creating two classes, which sounds tedious already, I know. But with the autocomplete feature in one of those extensions above, you only need one word to make all that appear. The same goes for the stateless. Apparently, since some widgets have so more than one required property, you can choose to autocomplete those property by typing `alt + space`. So if you know what you're doing, it's not exactly that cumbersome.

Take the `import` line for example, you mostly need to import material.dart, foundation.dart, intl.dart, if you're working on an android project and depending on what you need. There's also autocomplete on those imports even if it comes from the dependencies you installed on your project, as well as the importing files you have made yourself, so you don't have to worry much about making typos.

Unfortunately, you have to import every bit of widget you need that is in a separate file. Unlike in Xcode, you can't access classes without importing them. The cool thing about the autocomplete feature in one of those extensions is that if you use a class/widget, and let the autocomplete do the work for you, it will automatically import that class/widget.

One line function. I think this also exists in Swift, but you might be curious to know if you see one of these `() => print('hi');`. It is simply like:

    () {
    	print('hi');
    }

For a previous Swift programmer, the transition will probably be a breeze. There are so many similar concepts even if words don't look the same. But the commas and semicolons are unforgiving. You also wouldn't like the way you need to import every image and font you need in your app in your pubspec.yaml file. **Update**: I have decided to come back to being an iOS developer because the syntax of Dart and the way things are handled in Flutter are just way too much. I don't like handling states with Flutter which is a very complicated process. And having to implement state management every time the UI needs changing is a really dreadful task.

For a previous Android developer, which I also used to be, well, Flutter is a way much easier way to develop apps. Though, coding with Flutter feels so much closer to Android development than UIKit. A huge reason why I find UIKit easier.

I have said so many bad things about Flutter now that I realized, but it simply isn't that hard to learn. Sure, there are so many new things that even I only just learned, but since most of the code I wrote is repetitive, it makes more and more sense as I go through with the Udemy course about Flutter. With the way things are, it is no doubt Flutter could be adapted by many developers in the future. Its lightweight-ness to develop is just something most mobile developers could wish for, but I wish it was more similar to Swift and UIKit.