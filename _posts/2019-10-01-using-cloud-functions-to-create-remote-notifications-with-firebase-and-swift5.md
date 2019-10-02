---
title: Using Cloud Functions To Create Remote Notifications With Firebase And Swift5
tags:
- remote
- notification
- swift
- cloud
- function
- firebase
- messaging
- featured
- APNs
image: assets/images/Notifications_Top_2x.png
categories: Tutorial Swift Notification
---

This is tested using Firebase Realtime Database, not with Firestore. Before all of these, I assume you have already been using the realtime database. More importantly, you already have the GoogleService-Info.plist in your Xcode project. It is important because it contains your bundle identifier. Without Firebase knowing your bundle identifier, you can't proceed to enabling the Cloud messaging. We're not doing what's in the picture. It's just a formality.

Install this pod libraries first in addition to your preinstalled libraries with Firebase.
```
pod 'Firebase/Analytics'
pod 'Firebase/Core'
pod 'Firebase/Messaging'
```

In your Xcode project's signing and capabilities, enable or add capabilities:
1. Background Modes -> Remote Notifications
2. Push Notifications

Let's create a certificate signing request by going to Keychain Access.

1. On the toolbar, click `Keychain Access` beside `File`
2. In this dropdown menu, Certificate Assistant -> `Request a Certificate From a Certificate Authority`
3. Enter your credentials into the text fields and choose `Save to disk`.

Then go to Apple Developer. For this, you need a paid developer account which costs $99. 

1. Open `Certificates, Identifiers & Profiles`
2. Navigate to `Identifiers`
3. Click on the current project you are working on.
4. Beside the Push Notifications, click `Configure`.
5. Depending on which environment you are, click `Create Certificate`
6. `Choose a file` -> Upload the CertificateSigningRequest.certSigningRequest you just created.
7. Continue and download aps_development.cer.

Once that is done, you will have a .cert file on your desktop. Now let's convert that to .p12. Still, using the Keychain Access.

1. File -> Import Items -> Select your .cert file
2. Drag that file to `System`, assuming you are in `login` in the `Keychains` sidebar.
3. Drag it back from `System` to `login`. You now have a duplicate, but you'll only need the most recent.
4. Select `My Certificates` in the Category.
5. Select the most recent certificate pertaining to your project, and right-click.
6. Click Export "Apple Development IOS Push Services: your.project..."
7. Make sure to export it in .p12 format.
8. It will then ask you to create a password to protect this certficate. Give it one and click `ok`.

In order to test this while under development, you need a Provisioning Profile for development to authorize your devices to run an app that is not yet published on the App Store.
1.  Open `Certificates, Identifiers & Profiles`
2.  Select `Profiles`
3.  Generate a Profile
4.  Select iOS App Development and continue
5.  Select your App ID and continue
6.  Select the iOS Development certificate of the App ID you have chosen in the previous step, then click Continue.
7.  Select the iOS devices that you want to include in the Provisioning Profile, then click Continue. Make sure to select all the devices you want to use for your testing.
8.  Name the provisioning profile whatever you want and generate.
9.  Download the provisioning profile and open it to install.
10.  If it crashes your Xcode, just go to your Xcode project settings -> Signing & Capabilities -> Disable Automatically Manage Signing -> Import the provisioning profile.

To enable the cloud messaging feature, go to https://console.cloud.google.com/ -> Search for your Firebase project and select it -> `APIs and Services` -> `+ ENABLE APIS AND SERVICES`-> Search for `Firebase Cloud Messaging API` -> click Enable.

In your Firebase console, navigate to `Project Settings` -> `Cloud Messaging` -> `iOS app configuration` -> `APNs Certificates` -> Upload the `.p12 file` here, then enter the password for this certificate.

That's all you need for now to allow `Cloud Messaging` on your iOS device. It's like a term for Remote Push Notifications. I was baffled at that at first.

Now this is the part where we detect changes in the firebase database. If you haven't installed the [firebase CLI][cli] yet, please do so.
1. Create a directory where you want to put the cloud functions code. In my case, I created `triggers` directory under my Xcode project directory. `Laundry City/triggers`
2. 'cd triggers`
3. In the terminal, type `firebase init functions`, but make sure you're logged in with firebase cli.
4. Choose javascript and type `y` as in yes to any remaining questions from the cli.
5. Open index.js and create your function such as below:

```
// index.js
const functions = require('firebase-functions');
const admin = require('firebase-admin');

admin.initializeApp();

exports.sendNewChangesNotification = functions.database.ref('houses/{house}/items/{item}/').onWrite((change, context) => {

    var topic = "Serenity";
		
    // A message that contains the notification that devices will receive	
    var message = {
      notification: {
        title: 'New Quantity',
        body: change.after.val().product + ' now has ' + change.after.val().tentative + ' new items.'
      }
    };

    // Using Cloud Messaging to create notification
    return admin.messaging().sendToTopic(topic, message).then(function (response) {
        console.log('Successfully sent message:', response);
        return null;
    }).catch(function (error) {
        throw new Error("Error sending message:", error);
    });
})
```

First thing to note here is, you won't start your `ref` with the Project ID.. Instead, the branch below it. The ones in the brackets are `ids`, which varies. This will detect any write or changes happening in that reference. It will catch the changes in the `change` parameter. Thereby, allowing access to the new tentative value in `change.after.val().tentative` from the reference `houses/1/items/2/tentative`. 1 and 2 here are just examples of IDs. `sendNewChangesNotification` is just a variable. You can change it however you like.

When everything is in place, go back to terminal and deploy it with `firebase deploy`. Now, you have a listener or a trigger whenever your items have been changed or added.

Your AppDelegate.swift should look like [this][messaging]. This firebase sample is as instructed in the [documentation][doc].

To send a notification, `admin.messaging.send` will do the trick. But you have to provide a `token`, `topic`, or a `condition`. `Token` is generated by the AppDelegate and is uploaded to Firebase automatically. If you specify a specific token, then only the device with that token will receive the notification. `Topic` is what its name suggests. To subscribe to a topic, add this to wherever you need to put in your Xcode project:

```
Messaging.messaging().subscribe(toTopic: "Serenity") { error in
          print("Subscribed to weather topic")
        }
```

`Condition` can be something like you want to send this to notification to subscribers of more than one topic. For example: `"'Serenity' in topics || 'Manor' in topics"`. Further [documentation][topics] about this.

[topics]: https://firebase.google.com/docs/cloud-messaging/ios/topic-messaging
[doc]: https://firebase.google.com/docs/cloud-messaging/ios/client
[messaging]: https://github.com/firebase/quickstart-ios/blob/master/messaging/MessagingExampleSwift/AppDelegate.swift
[cli]: https://firebase.google.com/docs/cli
