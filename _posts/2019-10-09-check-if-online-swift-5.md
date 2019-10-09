---
image: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQBXD5o0JeyxP-5MmwSZO4lOrONruMPlZmrTRWonLxYsgx1iPYH
title: Check If Online Swift 5
tags:
- ''
- online
- swift
- check
- internet
- connection
- available
- offline
categories: Tutorial Swift
---

Thanks to [Paul Hudson @twostraws][ph] for the informative knowledge on how to check for internet connection. Let me remind you that this solution is available since iOS 12.0. WWDC in June 2018 introduced the Network framework available from iOS 12 onwards which includes the NWPathMonitor class, which we will be using in this tutorial. Here's a straight forward example of the basic implementation on how to check for internet connection.

```
import Network
private let monitor = NWPathMonitor()
monitor.pathUpdateHandler = { path in
    if path.status == .satisfied {
        print("Online")
    } else {
        print("Offline)
    }
```
 It's so much shorter than the previous baffling solutions that you can find on stackoverflow. [Previous solutions][previous] had to be particular about the connectivity. If it's coming from a WiFi, 4G, 3G or cellular data. My previous way of checking the internet connection was to use `Alamofire`'s network reachability feature which was way simpler than the other solutions, but I added a library that only uses this one feature. Hence, longer build time for something thatI didn't necessarily need. Now you don't even have to worry for the most part. It's just that simple code that allows you to check for internet connectivity.

Now, I created something to allow me to check for the internet connection in different parts of my project without having to `import Network` all the damn time.

```
func online(completion: @escaping (Bool) -> Void) {
    monitor.pathUpdateHandler = { path in
      if path.status == .satisfied {
        completion(true)
      } else {
        self.showError(message: "Offline! :(")
        completion(false)
      }
    }
}
```

I created this inside a singleton class which allows me to call this method simply, anywhere, like this:

```
Singleton.sharedInstance.online { (err) in
    guard err == false else { return }
		
    // Do whatever online
}
```

[ph]: https://www.hackingwithswift.com/example-code/networking/how-to-check-for-internet-connectivity-using-nwpathmonitor
[previous]: https://stackoverflow.com/questions/30743408/check-for-internet-connection-with-swift
