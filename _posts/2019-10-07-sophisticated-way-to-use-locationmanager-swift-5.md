---
image: https://i.stack.imgur.com/hDiMB.png
title: Sophisticated Way To Use LocationManager Swift 5
tags:
- ''
- location
- manager
- swift
- data
- corelocation
- operation
- queue
categories: Tutorial Swift CoreLocation
---

This is the most elegant solution I found on how to retrieve location with CoreLocation.

```
import CoreLocation
class ViewController: UIViewController {
    
    private let locationManager = CLLocationManager()
    private let operationQueue = OperationQueue()

    override func viewDidLoad() {
		super.viewDidLoad()
	
		operationQueue.isSuspended = true
		runLocationBlock {
			if let location = self.locationManager.location {
			}
		}
}
```
1. We must create a Location Manager first and foremost.
2. We created an OperationQueue even though we only have one operation to execute, which is `runLocationBlock`.
3. Suspend the operation or pause first to check if we are allowed to request location data.
4. Whatever is in the `runLocationBlock` will only run if `operationQueue` is unsuspended but rest assured this function is already running.

```
extension ViewController: CLLocationManagerDelegate {
    private func configureLocationManager() {
        locationManager.desiredAccuracy = kCLLocationAccuracyBest
        locationManager.delegate = self
				
        // requestLocation will take 10 seconds to run, but only requests location just once
        locationManager.requestLocation()
				
        // startUpdatingLocation will fire up immediately
        // locationManager.startUpdatingLocation()
    }
		
    func locationManager(_ manager: CLLocationManager, didFailWithError error: Error) {
        print("error")
    }
    
    func locationManager(_ manager: CLLocationManager, didChangeAuthorization status: CLAuthorizationStatus) {
        
        //If we're authorized to use location services, run all operations in the queue
        // otherwise if we were denied access or restricted or location is not determined, cancel the operations
				
        if(status == .authorizedWhenInUse || status == .authorizedAlways) {
            self.operationQueue.isSuspended = false
        }else if(status == .denied || status == .restricted || status == .notDetermined){
            self.operationQueue.cancelAllOperations()
        }
    }
    
    func runLocationBlock(callback: @escaping () -> ()) {
        
        //Get the current authorization status
        let authState = CLLocationManager.authorizationStatus()
        
        //If we have permissions, start executing the commands immediately
        // otherwise request permission
        DispatchQueue.main.async {
            if(authState == .authorizedAlways || authState == .authorizedWhenInUse) {
                self.configureLocationManager()
                self.operationQueue.isSuspended = false
            }else{
                //Request permission
                self.locationManager.requestWhenInUseAuthorization()
            }
        }
        
        //Create a closure with the callback function so we can add it to the operationQueue
        let block = {
            DispatchQueue.main.async {
                callback()
            }
        }
        
        //Add block to the queue to be executed asynchronously
        self.operationQueue.addOperation(block)
    }
}
```
## `RunLocationBlock`
1. We know for one thing that `runLocationBlock`'s operations is/are set suspended in the `ViewController`.
2. We want to know the authorization status first.
3. If we are authorized, then let's configure the `locationManager` second then unsuspend, next, to allow our operationQueue to run all the blocks of operations it cointains.
4. If we are not authorized, a request will be shown to the user using the app.
5. The `block` will contain the callback, which is whatever you put inside the `runLocationBlock { }`.

## `DidChangeAuthorization`
1. This is just like what we've seen inside `runLocationBlock`, but we don't request for authorization again. Instead, when denied/restricted or if the location was not determined, then the `operationQueue` will not run operations that were added to it.
2. This is only triggered after `self.locationManager.requestWhenInUseAuthorization()` that you have seen inside the `RunLocationBlock`.


##  `ConfigureLocationManager`
1. `locationManager.delegate = self` this is the most essential part of setting up our locationManager. This allows our `ViewController` to access protocol methods from `CLLocationManagerDelegate` like `didChangeAuthorization`.
2. If the desiredAccuracy is not set, its default is `kCLLocationAccuracyBest` for iOS/Mac OS. For watchOS, `kCLLocationAccuracyHundredMeters`. This is how you want the location data to be as accurate. It is important to note that higher accuracy will require longer time to retrieve location data.
	1. 	`kCLLocationAccuracyKilometer ` - within a kilometer
	2. 	`kCLLocationAccuracyThreeKilometers` -within 3 kilometers. The higher, the better conservation of energy.
	3. 	`kCLLocationAccuracyBestForNavigation`- usually used in navigation apps. Least energy efficient.
			1. 	locationManager.activityType = .automotiveNavigation
			2. 	locationManager.distanceFilter = 0
	4. `kCLLocationAccuracyNearestTenMeters` - less energy efficient.
3. If you use `requestLocation()`, it will only gather location data once. However, this takes 10 seconds as opposed to `startUpdatingLocation()`, which fires immediately.
