---
title: Weak Self vs Unowned Self
image: https://i.ytimg.com/vi/q0-DIJszYRo/maxresdefault.jpg
tags:
- ''
- swift
- tutorial
- unowned
- weak
- self
- memory
- leak
- deinit
categories: Tutorial Swift
---

Most articles regarding this are overwhelming and I'm about to make it easier. This is based on Brian Voong's tutorial. I urge you to watch the video too because, essentially, that is what I'm basing this article on.

<iframe width="560" height="315" src="https://www.youtube.com/embed/q0-DIJszYRo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 
In his tutorial, there are two views. One trying to present the other, the other view has a button to go back to the first view. 

His objective in this tutorial was to simply demonstrate how the presented view is removed from memory when popping back to the previous view by the use of `[weak self]` or by `[unowned self]`. His closures are a bit too advanced to understand, so let's make it more conceptual than actual.
 
Say this is the presented view. 
```
class 2ndView: View {
	deinit {
		print("The memory used by this view is being released")
	}
	
	self.viewDidLoad (){
		closure {() in
			self.alert()
		}
	}
	
	func alert() {...}
}
```

By doing it like this, the `self` in the closure, by default, holds a strong reference to itself, being the 2ndView. And when it does that, it simply makes it difficult to relase the 2ndView from the memory when going back to the 1stView. And by `strong`, it means the `deinit` won't be working here.

In order to make the reference to 2ndView less strong, we use weak by replacing our closure to this:

```
closure { [weak self] () in
	self?.alert()
}
```

Notice that we made the `self` an optional. By using weak, we can't guarantee that `self` always has a value. If we are certain that `self` always returns a value, then we can use `unowned`.

```
closure { [unowned self] () in
	self.alert()
}
```

Both solves the same memory problem and will print out `The memory used by this view is being released` every time the 2ndView is unloaded.

This can also be done for variables pointing to classes. Let's have an example of the 1stView:

```
class 1stView: View {
	weak var 2ndView: View?
	
	self.viewDidLoad() {
		navigationController?.pushViewController(2ndView(), animated: true)
	}
}

1stView = nil
// The memory used by this view is being released
```

After unloading the 1stView, our 2ndView is also unloaded ang gives us the printed message from its `deinit`.

Here's a [post by Antoine v.d. SwiftLee 🚀][avd] which explains what ARC does behind the curtains.

[avd]: https://www.avanderlee.com/swift/weak-self/
