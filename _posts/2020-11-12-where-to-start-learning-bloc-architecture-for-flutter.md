---
date: 2020-11-12 13:21:27 +00:00
title: 'Where To Start Learning BloC Architecture for Flutter '
categories:
- BloC
- Flutter
- Guide
tags:
- MVVM
- getx
- provider
- ios
- android
- flutter
- state management
- architecture
- bloc
image: assets/images/image_2020-11-12_212335.png

---
If I'm not mistaken, you, yes you, probably are using a different state management before trying to learn BloC. I did too. I knew Provider first, then GetX. GetX is the holy grail, but are we ever going to get a job that uses GetX? Probably not.

So I come looking for a state management that is probably more preferred by companies. I skimmed through Facebook, Reddit, YouTube videos, blogs, and other mediums to know which exactly would be the preferred state management. According to the people of the internet, Google prefers the use of Provider, having a newer edition called, Riverpod. You can still use the old Provider though.

But many also said BloC, which is best for large projects but an overkill for tiny projects. I can't imagine people who hire Flutter developers would build tiny projects. It's also good for testing too. And since I'm going to be learning about testing soon when I'm done with my project, I'll have to learn BloC.

I know it's a ton of boilerplate and it looks daunting. At this point, I haven't even fully grasp it but here's some references that will make you slowly learn it. Don't expect an overnight success on trying to understand it, just trust the process.

First, to learn BloC, is to be able to know how to use it. You need something real. You need to know how a simple state management is slowly transformed into a full-fledged bloc architecture. I say architecture because it's not only a state management tool, but it forces you to follow MVVM architecture, which is what I'm also trying to learn at the moment. If you come from GetX, then I completely understand if you'll be frustrated with all the new concepts you're about to learn. One video is not gonna cut it. Even Reso Coder can't manage to make a complete noob into pro with just one video. Though, I really appreciate Reso Coder's vids. But to start out learning BloC from him would be confusing. Still, I'd recommend watching his BloC tutorials when you've finally grasped some concepts.

Okay, enough of that whole miserable outlook on BloC. Let me just show you this.

<iframe width="560" height="315" src="https://www.youtube.com/embed/jIoWkct6_EM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

CodeX is another Indian YouTuber who makes tutorials, but don't despair. His accent is actually quite bearable compared to others. This video from him is a two part tutorial. It's really good because you'll get the core principles of where BloC actually stems from in a way more understandable manner than any other references I will point you to. His tutorial goes through concepts one by one without dropping bombs of new concepts that you have barely encountered in your development journey. It's important to note that he did not use any bloc related dependencies in his two part tutorial, only that, he shows how this BloC pattern came about. So, remember, start with CodeX' tutorial first because the rest of the BloC tutorials will make sense if you finish his' first. He, then, did not continue making any more BloC tutorials, instead he proceeded to making GetX tutorials. 

Next would be from this new channel that only has BloC tutorials. At this time of writing, he only has 7 videos in his channel and it's all about BloC. You won't see a face of him, but the visuals are so elaborate. He has not plans to write any article or create a blog for his channel, but I think his videos suffice. He also plans to make more videos regarding the recently updated flutter_bloc library.

<iframe width="560" height="315" src="https://www.youtube.com/embed/_7Mh66FFSNg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

His videos start to get longer at episode 2 onward which can reach up to almost 30 minutes but it's well worth it. His tutorials has a more theoretical way of teaching BloC, but it's not boring I promise you. You might just have to rewatch those videos because it's a little fast for someone who might not fully grasp BloC from CodeX yet, and that, BloC is packed with different kinds widgets and boilerplate that is a must know. There are also code examples, but he doesn't go writing an app to fully visualize those examples. But then again, his explanations are superb.