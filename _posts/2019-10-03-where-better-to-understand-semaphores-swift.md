---
title: Where Better To Understand Semaphores (Swift)
date: 2019-10-03 00:00:00 Z
categories:
- Tutorial
- Swift
tags:
- ''
- featured
- semaphore
- swift
- counter
- dispatchqueue
- centreal
- grand
- central
- dispatch
image: assets/images/1_7aP7xcLcB6cJ9wySieNNCA.gif
---

Thanks to Roy Kronenfeld's [Medium post][roy] for this explanation of how to use a semaphore, specifically, utilizing the counter: `let semaphore = DispatchSemaphore(value: 0)`. Another [tutorial][lbat] was made by `Let's Build That App` but semaphore wasn't covered in great detail, and I think it's best if all iOS developers dive into the semaphore counters the way Roy did.

In Roy's tutorial, it's more on semaphore counters. But if you want to see semaphore in action, kindly refer to my [previous post][previous] where it is applied on a `Futures` function.

His sample analogy was most brilliant. If you don't need to access a shared resource, feel free to use DispatchQueue. If you do, then you should consider `Semaphores`. The shared resource here is the `iPad` that 3 kids share. To cut the story short, there is only one iPad and our counter needs a value of only `1`.  Only one iPad can be shared among 3 kids at a time.

Another example of his is downloading songs. He wants 15 songs to be downloaded and he wants  to download 3 songs at a time. Therefore, you've guessed it, the counter is set to `3`. The shared resource here is the downloading processes or threads, not the song.

You can use the semaphore in 2 different ways by [Wain from Stackoverflow][wain] :

1. To limit the number of concurrent operations / requests / usages. In this case you start the semaphore at a positive value, like 4. The users each call wait and if resource is available they are allowed to continue. If not they are blocked. When each has finished with the resource they call signal. Just like the two examples given above.
2. To say when work or a resource is ready and we want to proceed to main thread next. In this case you start the semaphore at 0. The creator calls signal when something is ready. Semaphores can be used when there’s an asynchronous API that you need to make synchronous, but you can’t modify it. The example and explanation below is from [Soroush Khanlou][sk]:

```
// on a background queue
let semaphore = DispatchSemaphore(value: 0)
doSomeExpensiveWorkAsynchronously(completionBlock: {
	semaphore.signal()
})
semaphore.wait()
//the expensive asynchronous work is now done after calling semaphore.wait()
```

Calling .wait() will block the main thread until .signal() is called. This means that .signal() must be called from a different thread, since the current thread is totally blocked. Further, you should never call .wait() from the main thread, only from background threads.

If you want a more elaborate explanation on how this counting algorithm works, head on to his [blog post][roy] for more.

[roy]: https://medium.com/@roykronenfeld/semaphores-in-swift-e296ea80f860
[previous]: /blog/where-to-learn-async-callback-and-futures/
[lbat]: https://www.youtube.com/watch?v=6rJN_ECd1XM
[wain]: https://stackoverflow.com/questions/37154877/creating-semaphore-with-initial-value-of-0-make-issues-with-execution
[sk]: http://khanlou.com/2016/04/the-GCD-handbook/
