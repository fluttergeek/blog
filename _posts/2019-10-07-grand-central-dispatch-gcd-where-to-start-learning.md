---
image: https://res.cloudinary.com/practicaldev/image/fetch/s--orob56V4--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://learningswift.brightdigit.com/wp-content/uploads/sites/2/2019/09/ParallelConcurrency.001-1024x768.jpeg
title: 'Grand Central Dispatch (GCD): Where To Start Learning'
categories: Reference Swift
tags:
- ''
- grand
- central
- dispatch
- dispatchqueue
- barrier
- ultimate
- guide
- asynchronous
- operation
- swiftui
- combine
- framework
---

I've been hearing and reading GCD for quite a while now. Grand Central Dispatch. Whoa! Big word! It's really just a concept of using threads. It's not a railway system, no. 
> GCD is an API provided by apple to allow you to manage concurrent operations in a smooth way, in order to avoid freezing of your application and keep it always responsive for users. - Jaafar Barek

[Swift Multi-Threading using GCD For Beginners][begin]. This is just a brief article with great visual aids to simply start off learning DispatchQueue.

When you're done with that, hold your breath and dive in two of these lakes. [Asynchronous Multi-Threaded Parallel World of Swift][async] and [Ultimate Grand Central Dispatch tutorial in Swift][ultimate]. 

The former will first walk you through processing concepts and many visuals on how to use DispatchQueue, Semaphores, and SwiftUI's combine in relation to asynchronous operations. 

The latter is almost the same but with less visuals and no `combine`. DispatchWorkItem and deadlocks is discussed which isn't found in the former. 

Both are really valuable resources for learning `GCD`. It takes quite a while to grasp this simple concept but with broad applications, so I recommend reading on both to consolidate that chunk of knowledge. It is really intimidating to learn, since most of the time, programmers only usually use `background` and `main`.

I bet these resources are good for interviews too.



[begin]: https://hackernoon.com/swift-multi-threading-using-gcd-for-beginners-2581b7aa21cb
[async]: https://dev.to/leogdion/asynchronous-multi-threaded-parallel-world-of-swift-4gjh
[ultimate]: https://theswiftdev.com/2018/07/10/ultimate-grand-central-dispatch-tutorial-in-swift/
