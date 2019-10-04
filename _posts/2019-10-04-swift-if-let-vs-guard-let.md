---
title: 'Swift: If Let vs. Guard Let'
image: https://i.ytimg.com/vi/P2YZu9MwLaM/maxresdefault.jpg
tags:
- ''
- swift
- language
categories: Tutorial Swift
---

This is a brief explanation for newbies. It's because when I was still a newbie, I didn't really understand how or when to use either of these two. I never touched them at all. If you want a more thorough explanation on this, check out the [Swift Language Guide][slg].

```
if john.residence?.printNumberOfRooms() != nil {
    print("good")
} else {
    print("bad")
}
```

Both if and guard checks whether the value of a variable could be nil. They are interchangeable depending on the implementation. Let's start with the code above. This is a primitive way of checking whether a variable/method is or returns `nil`. It is closer to how `guard` works than `if let`.

```
if let firstRoomName = john.residence?[0].name {
    print(firstRoomName)
} else {
    print("bad.")
}
```

It is important to note here that `if let` only allows you to acces the new variable `firstRoomName` inside the first bracket. That is if  john.residence?[0].name `optional` assigns a non-nil value in our new variable.

Outside this `if let` conditional, `firstRoomName` cannot be accessed.

```
guard let firstRoomName = john.residence?[0].name else {
    print("bad.")
    return
}

print(firstRoomName)
```

`guard let` is just a smart way of making sure a new variable, `firstRoomName`, will not be accessible anywhere if it is `nil`. If `firstRoomName` happens to be assigned a `nil` value, then "bad" will print and will escape the function where this piece of conditional is located, never allowing to pass `firstRoomName` to anyone who wants to access its value. The code will not be able to reach the `print` line anymore.

However, if `john.residence?[0].name` hold say a string value, then all is good. `firstRoomName` will now have an unwrapped value of `john.residence?[0].name`.

[slg]: https://docs.swift.org/swift-book/LanguageGuide/OptionalChaining.html
