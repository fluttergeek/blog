---
title: Context Menu in SwiftUI
date: 2019-10-15 00:00:00 Z
categories:
- Tutorial
- SwiftUI
tags:
- ''
- context
- menu
- swiftui
image: assets/images/Screen%20Shot%202019-10-15%20at%209.16.42%20AM.png
---

This is one of the coolest things you'd see in an iOS app. Context Menus, like the one showed in the picture, was something that I thought could only happen with App Icons. It turns out they work within the app too.

It simply works like alertsheet. Like alertsheet, it is a collection of buttons but now with images attached to them too. The action is also encapsulated within the button. The best part is you no longer have to use a nasty looking closure to make that happen.
```
import SwiftUI

struct ContentView: View {
    var body: some View {
        Image("pic").resizable().frame(height: 300)
            .cornerRadius(20).padding()
            .contextMenu {
                VStack {
                    Button(action: {
                        print("save")
                    }) {
                        HStack {
                            Image(systemName: "folder.fill")
                            Text("Save To Gallery")
                        }
                    }
                    Button(action: {
                        print("send")
                    }) {
                        HStack {
                            Image(systemName: "paperplane.fill")
                            Text("Send")
                        }
                    }
                }
            }
        }
    }
}
```

You could not only use it in images, but also with labels among other things. These things are probably not solid yet, so they might crash your app.

Now everything looks cleaner than building one on UIKit. It's just that these brackets kind of make me dizzy. But don't fret, you can compartmentalize these things with your programming prowess.

Example was taken from `KAVSOFT`:

<iframe width="560" height="315" src="https://www.youtube.com/embed/NgXHemwSFIQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
