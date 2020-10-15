---
date: 2020-10-14 20:30:30 +00:00
title: I'm Choosing GetX Over Provider/Riverpod
categories:
- Flutter
tags:
- state management
- provider
- riverpod
- getx
image: assets/images/1_palbtvqaimmohq1xbbfy9w.png

---
I've just come back from aspiring to become a respectable video editor. Sadly, video editing isn't my passion. It is way easier, but it doesn't keep me up at night wanting to quickly solve a problem. It's been several months since I tested out Flutter. Back then, my knowledge was only enough to get me to finish on a project I was building. Granted, it was mind boggling to learn as I built my personal project. The state management I was using was Provider, and while there were Bloc, Mobx, and what not, I still chose provider because it was what people were recommending back then. I guess I might have noticed GetX too, but there just wasn't that much buzz about it.

I've come across GetX in the YouTube recommendations while I was trying to wrap my head around the new Riverpod. I thought it was going to be much easier, learning this Riverpod thingy as compared to learning Provider. I felt like starting from scratch relearning this state management again. This damn state management complexity. I had to browse through Provider again so I can see what changed and also the similarity between Riverpod and Provider still got me confused what changed. I've been watching tutorials about it trying to understand all of it before starting yet another personal project using Flutter. It felt like I was learning Provider all over again. 

So, now that I notice GetX again, I mean, why the hell not start digging in because this could be way easier than learning Riverpod.

True enough, it was a breeze to not have to go through notifiers and the mundane way of setting up navigation routes. It looks so much simpler and it covers streams too. I was still in the process of learning streams as well, but I guess I understand it better now that I've gained some proficiency in GetX.

Basically, streams are continues influx of data, which you might often use in a messaging feature or notifications. That's when you use GetX or Obx. Otherwise, for not so reactive states, like when you switch tabs and you need to keep the index of that tab in a state, you can use GetBuilder for that instead. These three GetX functions could be very easily confused at first since their syntax look almost alike, but not as confusing as using StreamBuilder, RxDart, Bloc pattern, Provider/Riverpod, and whatever else there may be.

GetX syntaxes are shorter, easier to understand, less notifiery bullshit, and it covers so much about state management already with such minimal code. I might never have to use stateful widgets any more, however, I still do when I'm lazy creating separate classes just for simple states. GetX has got me back on MVC, like when I did Rails back then. I guess you could also do it in a different architecture.