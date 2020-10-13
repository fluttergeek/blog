---
title: MapKit Theme Style Overlay Swift 5
date: 2019-10-04 00:00:00 Z
categories:
- Tutorial
- MapKit
- Swift
tags:
- ''
- featured
- swift
- mapkit
- UIKit
- mapbox
- googlemap
image: assets/images/Simulator%20Screen%20Shot%20-%20iPhone%208%20Plus%20-%202019-10-04%20at%2020.23.31.png
---

You don't want to use MapBox or GoogleMap? Me too. I don't want to pay for something that isn't going to be much different. For now, I don't see myself using their extra features, whatever those might be.

I've been looking for a workaround for this for a few months now. I must have overlooked this [post][mappost] on Medium. It's now an outdated solution, but I found a way for it to work. Thanks to Fernando Martín Ortiz for that [post][mappost].

There's this pod, made by Fernando himself, that you need to install.

```
pod "MapKitGoogleStyler"
```

Wherever you're using the map view, you need to import this library: `import MapKitGoogleStyler`. And this is the sample code that contains a single change from Fernando's view controller that shows the map view.

```
import UIKit
import MapKit
import MapKitGoogleStyler

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        guard let overlayFileURLString = Bundle.main.path(forResource: "overlay", ofType: "json") else {
                return
        }
        let overlayFileURL = URL(fileURLWithPath: overlayFileURLString)
        
        // After that, you can create the tile overlay using MapKitGoogleStyler
        guard let tileOverlay = try? MapKitGoogleStyler.buildOverlay(with: overlayFileURL) else {
            return
        }
        
        // And finally add it to your MKMapView
        mapView.addOverlay(tileOverlay)
    }

    func mapView(_ mapView: MKMapView, rendererFor overlay: MKOverlay) -> MKOverlayRenderer {
            // This is the final step. This code can be copied and pasted into your project
            // without thinking on it so much. It simply instantiates a MKTileOverlayRenderer
            // for displaying the tile overlay.
            if let tileOverlay = overlay as? MKTileOverlay {
                return MKTileOverlayRenderer(tileOverlay: tileOverlay)
            } else {
                return MKOverlayRenderer(overlay: overlay)
            }
    }
}
```

You might run into an error because the pod is a bit outdated, but no worries. All you have to do is find that GoogleStyle.swift and replace it with [this][git].

Notice that we need a json file named `overlay`. It's where our theme's color scheme lies, but make sure you've configured the `json` file this way. Otherwise, your json will not be found by `Bundle.main.path(forResource: "overlay", ofType: "json") `. 

![json](/blog/assets/images/Screen%20Shot%202019-10-04%20at%209.22.14%20PM.png)

You may be wondering, where do I get such theme? Did I make those myself?

No, silly! I copied that JSON file. There's actually tons of those themes out there. I just got mine from [Snazzy Maps][snazzy].


[mappost]: https://medium.com/@ortizfernandomartin/customize-mapkits-mkmapview-with-google-maps-styling-wizard-a5dcc095e19f
[git]: https://github.com/fluttergeek/StyleMapKit/blob/master/StyleMapKit/GoogleStyle.swift
[snazzy]: https://snazzymaps.com/style/282895/xemeneies-pou
