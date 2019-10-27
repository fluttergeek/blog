---
title: Why I Prefer Cloud Firestore Than Realtime Database
date: 2019-10-02 00:00:00 Z
categories:
- Tutorial
- Firestore
- Firebase
tags:
- ''
- firestore
- firebase
- database
- collections
- documents
- Xcode
- swift
image: assets/images/update.jpg
---

This is just a brief insight on why I prefer using Firestore. At first, I was hesitant to dive into Firestore, because I thought there wasn't really much difference. I saw videos and tutorials of Firestore and it baffled me even more. So, I stayed with Firebase Realtime Database for a little long while, until I started with another Xcode project. I delved into why Firestore might be a better fit.

Initially, all I understood was Firebase RD will charge you depending on bandwidth usage, while Firestore will charge on writes and reads. I was intimidated by that. The key is to make the writes and reads as minimal as possible to lessen the cost.

In my previous personal project, I used Firebase RD. I have a web app for it and iOS app. Both coded by me. I was wondering why it always took so long to read from Firebase. Then I realized, I created a tree structure and whenever I tried to access the root's value, it grabs all the branches along with it making my succeeding read queries redundant. It loads a very heavy amount of information as my database grew. That's how I came to a decision to start using Firestore.

Collections. Documents. You must've read those before. It's confusing, I know. That's how Firestore works. Think of it this way. `Collections` are Folders, and `Documents` are files that can contain a pointer to a sub Collection. Documents contain the attributes and its values. Collections only contain Documents.

For example:

* Customer (`Collection`)
	* Jane (`Document`)
		* address: "In the woods"
		* age: 16
		
		* Orders (sub `Collection`)
			* 2019-1-19 (`Document`)
				* item: "Pizza"

I hope you see the pattern now. `Collections` always contain `Documents`, and `Documents` can contain `Collections`. 

What makes it better than Firebase DB?

Well, whenever you reference to collection("`Customer`").document("`Jane`"), you won't be including `Orders` in your read. Thereby, reducing the load. I guess you can say it's faster this way. You only get to access the `address` and `age` of Jane. If you want to get her orders as well, then reference to collection("`Customer`").document("`Jane`").collection("`Orders`"). You can loop through all her orders thereafter.

No matter how deep your tree becomes with Firestore, you won't be able to directly access its sub collections and that cuts down a lot of loading time and heaviness.
