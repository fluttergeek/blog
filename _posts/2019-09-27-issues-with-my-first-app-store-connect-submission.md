---
title: Issues With My First App Store Connect Submission
image: assets/images/Screen%20Shot%202019-09-25%20at%208.43.21%20PM.png
categories:
- Publish
tags:
- featured
- app
- connect
- itunes
- store
- submission
- capabilities
- uiwebview
- deprecated
- api
- support
- url
- FacebookCore
- notifications
---

It was my first app submission to the app store. I mean, well, I hadn't submitted it for review yet, but it already detected a couple of issues that needs dealing with. I uploaded it to app connect for 6 times until I got it right. 

The first issue I had to deal with only needed a little bit of tweaking in my project's `Signing and Capabilities`. From there, I added a Capability known as 'Push Notifications'.

The second one is because of a deprecated API. I tried updating a cocoapod library "Firebase/Auth" after finding out that this utilizes UIWebView. For some reason, I can't get update working on a single pod. Prior to this, I thought Facebook Core and Facebook Login were the culprits, knowing those would open a UIWebView for me to sign into Facebook. I guess I found a fix on Github issues, which helped other developers with what might have been a similar problem.

```
pod 'FacebookCore', '~> 0.9.0'
pod 'FacebookLogin', '~> 0.9.0'
```

After trying this out, it didn't actually help with the second issue. Until, I finally decided to  update all the pods with just a simple
```
pod update
```

I didn't know most of my pods were actually not up-to-date until I did that. Voila, I didn't get another warning after uploading my project to App Connect. 

There were just a few more things that needed filling up. Below are things that needed to be filled:

1. Support URL
	
	It would come as a surprise to you that you actually might need something like this. I mean, it surprised me too. I didn't fully grasp what it meant even after  hovering on the question mark to reveal insights of what this might be. From what I've gathered in teamtreehouse.com, "its purpose is to allow users to get in contact with you if they experience problems, desire new features or want to ask questions about your app." Hence, I just used my portfolio as the link.
	
2. Build
  
	This is an obvious requirement. And just to bring more light into it, because I didn't know how it worked at first. Every time you make changes in the code or anywhere in your project, you need to change the build number. So, after having dealt with those warning issues above, I had to increase the build number in project settings, otherwise I couldn't upload a project with an already existing build number in the app connect.

3. Rating

	This I had no idea too. I initially thought `why would I be rating stars of my own app?` Stupid, right? Well, just click the edit button beside it and you'll have a change of perspective.

 	![ratings](/blog/assets/images/Screen%20Shot%202019-09-27%20at%204.09.36%20PM.png)
	
4. Copyright

	I'm not running a company, so I also googled what I could possibly put here. I was wondering if I could actually use `2019 iOS Junkie`, but maybe not. So, I just used my name instead. 
	
5. General App Information and Contact Information

	I had to fill out all details with fields referring to name, address, contact number, and email address. Otherwise, it will turn red and not submit for review when you have to.
	
## Post submission

I encountered a rejection. Yes, you can't have all the fun yet. They sent me this email

![location](/blog/assets/images/Screen%20Shot%202019-09-27%20at%2010.45.05%20AM.png)

Below it was a screenshot of where in my app particularly went wrong. I changed my Information Property List with just one line of sentence that has to do with my location privacy usage to `Laundry City would like to use your current location to display it on the app, also for directions and estimations of distance. It is secure and private.` It's much longer and I hope it's worth it.
