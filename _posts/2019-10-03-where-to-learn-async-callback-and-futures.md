---
title: Where To Learn Async Callback and Futures
image: assets/images/reactivecocoa-functional-reactive-programming-concepts-in-ios-11-638.jpg
categories: Tutorial Swift
tags:
- ''
- async
- callback
- hell
- futures
- swift
- algorithm
- advanced
- await
---

One of my favorite online tutors is Let's Build That App. He's really good with his YouTube thing going on. Makes me understand a lot of Swift stuff that I never imagined to be digging into. Most of this article is coming from his tutorial and I've also included his video in case anyone's interested.

Async is concurrent. It does not wait for something to finish for it to start working. When you hear async, think low priority, background, and fetching data. That much I know. 

Fernando Martín Ortiz is another iOS developer who's talked about this. The callback hell he says. Callback hell looks exactly like our picture above. We must refrain from using callbacks with our async functions to avoid spaghetti like codes. Instead, like Let's Build That App, use `Futures`. Whatever the hell that means. Kidding aside, it actually means that it has to `return` something in the future instead of making a `callback`. Read more about his thoughts on that [here][managing]. 

Now let me dive into Let's Build That App's (LBTA) precious little code from his video tutorial. In this video, he shows how to convert a callback async function into a future async function. There's a lot of code here that'll overwhelm a newbie.

<iframe width="560" height="315" src="https://www.youtube.com/embed/JHqxmBFrWl8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```
enum NetworkError: Error {
    case url
    case statusCode
    case standard
}
```

Let's start with the simplest tiniest detail that we have to wrap our brains with. If you're a newbie, you might be intimidated from using enums. I know I do. `NetworkError` is just a name here and you can change it however you like. It inherits an `Error` which has no requirements of its own, you can declare conformance on any custom type you create. Hence, url, statusCode, and standard are just made up by LBTA. You can add more cases of your own words. In the end, these cases will be printed as strings anyway. For example, case url when printed `print(NetworkError.url)` will return "url". 

```
func fetchSomethingAsyncAwait(url: String) throws -> Data? {
    guard let dummyURL = URL(string: url) else {
        throw NetworkError.url
    }
    
    var data: Data?
    var response: URLResponse?
    var error: Error?
    
    // Semaphore
    let semaphore = DispatchSemaphore(value: 0)
		
    URLSession.shared.dataTask(with: dummyURL) { (d, r, e) in
        data = d
        response = r
        error = e
        
        semaphore.signal()
    }.resume()
    
    // this will return a result, doing this will just get rid of the warning
    // but its still functional without assigning it to an underscore
    _ = semaphore.wait(timeout: .distantFuture)
    
    if let httpURLResponse = response as? HTTPURLResponse, httpURLResponse.statusCode > 300 {
        throw NetworkError.statusCode
    }
    
    if error != nil {
        throw NetworkError.standard
    }
    
    return data
}
```

This is a `Future`. You know when you see one because it needs to return something, `Data?`. This is a function that can throw an error. So every time you use this function, you must always put it inside a do `try` catch. 

```
do {
    let data = `try` fetchSomethingAsyncAwait(url: "http://google.com")
} catch {
    print("Failed to Fetch stuff: ", error)
    return
}
```
 
If this catches an error, it might print something like "Failed to Fetch stuff: statusCode". 

But the most intriguing part of his code is the semaphore. He scheduled the return to execute after the URLSession has finished its task. If there was no semaphore, then his data might return nil. That's because he didn't initialize data when he first created it and it is an optional. With this semaphore superpower, he was able to ask the main thread to wait until a `signal()` has been found in the distant future. URLSession is the async here, and it is making another thread in the background. Once URLSession's task reached til the end of its closure, the signal will ask the main thread to continue. `if let httpURLResponse = response as? HTTPURLResponse, httpURLResponse.statusCode > 300` is the next thing after coming back to the main thread, then the `if error != nil`. This way, if no error has been thrown, the `return data` will surely have a value to return, otherwise, it returns nil.


[managing]: https://medium.com/ios-os-x-development/managing-async-code-in-swift-d7be44cae89f#targetText=You%20call%20that%20function%20and,resolved%2C%20is%20commonly%20called%20then.
