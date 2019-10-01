---
title: "@State to @ObjectBinding Modifier"
image: assets/images/maxresdefault.jpg
tags:
- ''
- state
- bindable
- object
- "@objectbinding"
- swiftui
categories: Tutorial SwiftUI
---

Notice how Scott changed from `@State` to `@ObjectBinding`. He started discussing BindableObject at 5:17. That's because you can only use `@State` with local properties in the struct view. Let's see his code one more time.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7sxdhunvSCg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


```
import Combine

class ListDataSource: BindableObject {
	var willChange = PassthroughSubject<Void, Never> ()
	
	var rowModels = [CustomRowModel] () {
		willSet {
			willChange.send()
		}
	}
}
```

In this tutorial, that's all the code we need. Just kidding. There's one more:

```
struct ContentView: View {
	@ObjectBinding var datasource = ListDataSource()
	
	// to access the rowModels array
	datasource.rowModels
}
```

Notice here that he used @ObjectBinding instead of @State. If you want a hold of the state of a class, then you must use @ObjectBinding instead and that class should inherit from `BindableObject` protocol, which requires that you have a `PassthroughSubject`. In order to use `PassthroughSubject`, import `Combine` first. You can substitude willChange to didChange. Heck, it's just a variable name. But if you do, also change the `willSet` to `didSet`. `willChange.send()` will notify the subscribed view, which in this case is `ContentView`, of any changes done to `rowModels` array.
