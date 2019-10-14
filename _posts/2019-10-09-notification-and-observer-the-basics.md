---
title: 'Notification And Observer: The Basics'
image: assets/images/1_Ca69vjmxQIdkOfW-O6hZaA.png
tags:
- ''
- featured
- swift
- notification
- observer
- push
- local
- remote
- trigger
categories: Tutorial Swift
---

This topic is not about Local Notifications or Push Notifications. It is about creating triggers. This is a straightforward snippet of how you call a trigger or the one-liner that notifies the observer:

```
// Trigger/Notifier
NotificationCenter.default.post(name: NSNotification.Name.init("done"), object: nil)
```

This will look for an observer that will allow you to send this trigger an action. It is looking for an observer named `done`. Let's create that too:

```
// Observer
NotificationCenter.default.addObserver(self, selector: #selector(sortAndStopLoading(n:)), name: NSNotification.Name.init("done"), object: nil)
```

Now this is something you'd probably find in the `viewDidLoad` of your ViewCotroller. Just to create an observation to catch the trigger with the name `done` and provides an action when the trigger has been called. Let's create that action too:

```
// Action
@objc func sortAndStopLoading(n: NSNotification) {
        done += 1
        
        if done == 2 {
            print("reached")
            self.sales = self.sales.sorted(by: {($0["number"] as! Int) > ($1["number"] as! Int)})
            self.stopLoading()
        }
    }
```

Never mind the content of my function. What is important here is you know how the selector's given function will actually look like. If you were wondering what is inside my function, it is simply just waiting for the trigger to call this function two times from maybe two different sources of thread that call the same trigger. When both threads are `done`, then we can sort the array's contents and tuck away the loader image.

If your ViewController is done `observing`, and we disappear from this view or something, we should remove this observer like so:

```
deinit {
    NotificationCenter.default.removeObserver(self, name: NSNotification.Name.init("done"), object: nil)
}
```

or like this 

```
override func viewDidDisappear(_ animated: Bool) {
    NotificationCenter.default.removeObserver(self, name: NSNotification.Name.init("done"), object: nil)
}
```

Don't you know? There's another way to make an observer that does not involve calling another function to make an action `action`. It calls the action inside its closure:

```
NotificationCenter.default.addObserver(forName: NSNotification.Name(rawValue: "done"), object: nil, queue: .main { [weak self] (notification) in
     done += 1
        
     if done == 2 {
        print("reached")
        self.sales = self.sales.sorted(by: {($0["number"] as! Int) > ($1["number"] as! Int)})
        self.stopLoading()
     }
}
```
