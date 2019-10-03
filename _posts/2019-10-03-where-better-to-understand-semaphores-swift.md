---
image: assets/images/1_7aP7xcLcB6cJ9wySieNNCA.gif
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
categories: Tutorial Swift
title: Where Better To Understand Semaphores (Swift)
---

Thanks to Roy Kronenfeld's [Medium post][roy] for this explanation.` Let's Build That App` made a brief tutorial on this topic but it was not explained much deeper and I think it's best if all iOS developers dive into the semaphore counters the way Roy did.

In Roy's tutorial, it's more on semaphore counters. But if you want to see semaphore in action, kindly refer to my [previous post][previous] where it is applied on a `Futures` function.

His sample analogy was most brilliant. If you don't need to access a shared resource, feel free to use DispatchQueue. If you do, then you should consider `Semaphores`. The shared resource here is the `iPad` that 3 kids share. To cut the story short, there is only one iPad and our counter needs a value of only `1`.  Only one iPad can be shared among 3 kids at a time.

Another example of his is downloading songs. He wants 15 songs to be downloaded and he wants  to download 3 songs at a time. Therefore, you've guessed it, the counter is set to `3`. The shared resource here is the downloading processes or threads, not the song.

If you want a more elaborate explanation on how this counting algorithm works, head on to his [blog post][roy] for more.

[roy]: https://medium.com/@roykronenfeld/semaphores-in-swift-e296ea80f860
[previous]: /blog/where-to-learn-async-callback-and-futures/
