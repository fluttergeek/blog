---
title: Basic Implementation of Local Notifications
date: 2019-10-08 00:00:00 Z
categories:
- Tutorial
- Swift
tags:
- ''
- local
- notification
- push
- remote
- UNUsernotification
- newbies
- basic
image: assets/images/Notifications_Top_2x.png
---

Let's get one thing straight now baby. Local notifications is not the same as `Push`/`Remote` notifications. It is also not the same as the Notification-Observer relationship which I will discuss [next][next]. They differ because local notifications don't need triggers coming from outside the app. Either way, you'll still need the `UNUserNotificationCenter`, and for that, you need to import `UserNotifications`. This is usually used in scheduling apps like alarm and todo-list.

```
// AppDelegate.swift
import UserNotifications

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        if #available(iOS 10.0, *) {
          // For iOS 10 display notification (sent via APNS)
          UNUserNotificationCenter.current().delegate = self

          let authOptions: UNAuthorizationOptions = [.alert, .badge, .sound]
          UNUserNotificationCenter.current().requestAuthorization(
            options: authOptions,
            completionHandler: {_, _ in })
        } else {
          let settings: UIUserNotificationSettings =
          UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
          application.registerUserNotificationSettings(settings)
        }
				
        return ApplicationDelegate.shared.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
}
```

We must have this in our AppDelegate.swift first. It asks permission from the user if he is okay yo receiving notifications from your app. That request will allow `alert, badge, and sound`. Depending on how much you want the user to experience the notification. You can omit sound or the other types for example: `[.alert, .badge]`. Next is the fun part, and you can choose to put it wherever your app needs it.

```
// 1. Preparing our notification content
let content = UNMutableNotificationContent()
content.title = "Hey kid"
content.body = "Where's your parents?"
content.sound = UNNotificationSound.default()
content.badge = 1

// 2. Preparing when our notification would fire
let date = Date().addingTimeInterval(5)
let dateComponents = Calendar.current.dateComponents([.year, .month, .day, .hour, .minute, .second], from: date)

// 3. Preparing what argument will trigger the notification to come up
let trigger = UNCalendarNotificationTrigger(dateMatching: dateComponents, repeats: false)

// 4. Combining all the preparations into one request
let uuidString = UUID().uuidString
let request = UNNotificationRequest(identifier: uuidString, content: content, trigger: trigger)

// 5. Finally, execute the request
let center = UNUserNotificationCenter.current()
center.add(request) { (err) in
    guard err == nil else { return }
    // Whatever you want to do after firing that badass notification
}
```

## This algorithm is broken down into five parts.
1. Create the content. That's what the user can see in your notification.
	1. The badge is the red circular thing on the top right corner of your app's icon with the number of how many notifications were fired up. 
2. Setting when to fire up the notification. If you print them out, this is how they'll look like:
	1. `"8 Oct 2019 at 2:53 AM"` // darw
	2. `year: 2019 month: 10 day: 8 hour: 2 minute: 53 second: 26 isLeapMonth: false ` // dateComponents
	3. You can create your date component this way to be more specific about the date: `DateComponents(year: year, month: month, day: day)`
3. Create a trigger and add the `when` to it. You can make it repeat its time interval by setting `repeat` to true.
4. It's possible that there may be many requests of triggers, hence we needed that `id`. UUID() is one hell of a one liner that can give you a random string. You can use it anywhere actually. It gives off something like this: `"31A7BADF-93F8-40A0-B642-DEDE7BD6179B\n"` if you print it.
5. Finally we may now make the request appear more tangible and we need the current instance of our notification center to allow such a thing to happen. 

## To add buttons on your notification
![buttons](https://www.appboy.com/blog/wp-content/uploads/2017/05/Push-Action-Buttons-Example-4-300x169.png)

1. Add something like this to step 1: `content.categoryIdentifier = "alarm"`
2. Add something like this before step 5:
```
let show = UNNotificationAction(identifier: "show", title: "Tell me more…", options: .foreground)
let category = UNNotificationCategory(identifier: "alarm", actions: [show], intentIdentifiers: [])
center.setNotificationCategories([category])
```

And make sure your ViewController conforms to `UNUserNotificationCenterDelegate` to catch the response coming off from the user's chosen action on the button.

```
func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    // pull out the buried userInfo dictionary
    let userInfo = response.notification.request.content.userInfo

    if let customData = userInfo["customData"] as? String {
        print("Custom data received: \(customData)")

        switch response.actionIdentifier {
        case UNNotificationDefaultActionIdentifier:
            // the user swiped to unlock
            print("Default identifier")

        case "show":
            // the user tapped our "show more info…" button
            print("Show more information…")
            break

        default:
            break
        }
    }

    // you must call the completion handler when you're done
    completionHandler()
}
```

Thanks to [CodeWithChris][chris] and [Paul Hudson @twostraws][paul] for making me understand how to actually do this.

[chris]: https://www.youtube.com/watch?v=JuqQUP0pnZY
[paul]: https://www.hackingwithswift.com/example-code/system/how-to-set-local-alerts-using-unnotificationcenter
[next]: /blog/notification-and-observer-the-basics/
