---
title: Custom Table Cell With SwiftUI
category: Tutorial
image: assets/images/Screen%20Shot%202019-09-28%20at%2010.32.57%20PM.png
---

First of all, thanks to Scott Smith for helping me understand this. I know it can be another learning curve to start learning SwiftUI if you've only begun diving into becoming an iOS developer. It's daunting at first, but I hope it can be simple too for many newbies. Now, I haven't gotten my hands on Mac OS Catalina yet, so I can't actually demonstrate my own version of this tutorial properly if I can't show you the canvas, which isn't available in Mojave yet, myself.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/7sxdhunvSCg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To make a table, add the `List()`  in your ContentView
```
struct ContentView : View {

	@State var data = ["Stevens", "Chloe", "Frank"]
	
	var body: some View {
		List(data, id: \.self) { item in
			CustomRow(name: item)
		}
	}
}
```

The id here is what confused me as a beginner. How Scott wrote and explained it is that `\.self` is meant to give identification automatically to each string from `data` itself. If you created a more comlex data structure, then you'll probably just have to specify the `id` in that structure and assign it a value of `UUID()`, which returns  unique id number.  `@State` on the other hand is something that will notify if there are any changes in the `data` array and update whichever component is using that variable. The `item` represents each item in the `data` array and it is being passed as an argument to another struct, `CustomRow`. It's pretty self explainable what can be done next to create the table cell here. We only have to put in the components we need to complete a single cell in this struct.
```
struct CustomRow : View {
	var name: String
	
	var body: some View {
		Text(name)
	}
}
```

If we didn't need to create a custom table cell, we need not create the `CustomRow` struct and just use `Text(item)` instead. In the body of our `CustomRow` is where you can throw in all the components you're going to need.

So, I thought to myself. That wasn't daunting at all. You no longer have to specify how many sections and rows, no need to get the cell identifier off from the storyboard, which reduces a hell lot of code to write. It's just that all UI designs you might have in mind will now have to be written in code which sucks.