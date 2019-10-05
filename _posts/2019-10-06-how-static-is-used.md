---
title: How Static Is Used
image: https://i.ytimg.com/vi/s2E5hVxQAZQ/hqdefault.jpg
tags:
- ''
- featured
- swift
- data
- structure
- static
- struct
- class
- construct
- type
categories: Tutorial Swift
---

Static is like creating a global variable. You'll see what I mean when you've watched Sean Allen's video tutorial.

<iframe width="560" height="315" src="https://www.youtube.com/embed/s2E5hVxQAZQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

There's just one hiccup here that might confuse the newbies. Sean used the term `Type` and the struct's name interchangeably.

Let's have at it one more time:

```
struct Video {
    static let creator = "iOS Junkie"
    
    var title: String
    var viewCount: Int
}

let tutorial = Video(title: "Static", viewCount: 1000)
```

The `Type` mentioned by Sean is the `Video` struct, which owns the creator. Say you access the creator via `Video.creator` without creating a `Video()` object, then it is a `type` and not an `instance` like tutorial. `tutorial` is assigned an instance of `Video()` object.

If you're curious whether `title` can be accessed this like this `Video.title`, then no. Even if it is pre-initialized a value, it will give out an error saying: `Instance member 'title' cannot be used on type 'Helper'`.

You'll want to use static for a reason like you might not want to create an object to access its properies or methods making it possible to directly access the struct `Vlog`'s properties without creating an object. Static can also be applied to a method, which allows you to do something like `Video.play()`.

It's also important to note that you can replace the creator's value just by assigning it a new value this way:
```
Video.creator = "Sean Allen"
```

The next time you access `Video.creator` or create an object with `Video()`, the creator will be `Sean Allen` by then. 

Now that we're talking about static, there's another way to access computed properties and methods of a construct without making an object. This is from Kilo Loco:

<iframe width="560" height="315" src="https://www.youtube.com/embed/CV0czLueGeI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```
class Helper {
    // Static - allowed in structs and classes
    static var staticStoredProperty = "Static Stored Property"
    static var staticComputedProperty: String {
        return "Static computed property"
    }
    
    static func staticMethod() {
        print("Static Method")
    }
    
    // Class - only allows within classes, not structs
    class var classComputedProperty: String {
        return "Class computed property"
    }
    
    class func classMethod() {
        print("class method")
    }
}

class SubHelper : Helper {
    override class var classComputedProperty: String {
        return "subClassed computed property"
    }
    
    override class func classMethod() {
        print("subclassed method")
    }
}
```

Aside from `static`, you can use `class` in exchange. No! Not the class construct. I'm talking about class type. It works almost similarly to static. However, you cannot store a property like you would with `static`. With `class`, you can do something that `static` too can't do. You can override computed properties and methods as you can see inside the the `SubHelper` class.

It is important to note that you cannot use the `class` type inside a `struct`, only inside classes.

These are all valid ways to access properties and methods given the code above:

```
Helper.staticStoredProperty
Helper.staticComputedProperty
Helper.staticMethod()

Helper.classComputedProperty
Helper.classMethod()

SubHelper.classComputedProperty
SubHelper.classMethod()
```
