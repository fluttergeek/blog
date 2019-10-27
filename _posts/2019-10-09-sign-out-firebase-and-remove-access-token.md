---
title: Sign Out Firebase And Remove Access Token
date: 2019-10-09 00:00:00 Z
categories:
- Tutorial
- Swift
- Firebase
tags:
- ''
- firebase
- sign
- out
- feature
- access
- token
- defer
- login
image: https://blog.hasura.io/content/images/2019/03/rawpixel-783430-unsplash-1.jpg
---

I call the `signOut()` to sign out, but there's really a lot of things going on inside my call. I'm not sure which is the correct way to do this yet, but what I'm trying to achieve is once I'm signed off, my login view will not automatically sign me back in. My login view checks if the access token contains a token, and if it does, then automatically sign me in and performSegue.

```
func logOut(completion:@escaping(_ errorOccured: Bool) -> Void)  {
    let firebaseAuth = Auth.auth()
    var err: Bool = true
    defer { completion(err) }
    do {
        try firebaseAuth.signOut()
        err = false
    } catch _ as NSError {
        err = true
    }
}
```

This is how my closure function looks like before I try to remove the access token in the next function that you'll see. 
* If it tried to `Auth.auth().signOut()` successfully,  there will be no error, otherwise there will be. 
* The error's boolean value will be passed to my completion block which will happen after the function scope will be exited.

```
func signOut() {
    logOut { (err) in
        guard err == false else {
            self.showError(message: "Unsuccessful logout :(")
            return
        }
        AccessToken.current = nil
        UserDefaults.standard.removePersistentDomain(forName: Bundle.main.bundleIdentifier!)
        AccessToken.refreshCurrentAccessToken( { (request, any, err) in
            if AccessToken.current != nil {} else {
                self.performSegue(withIdentifier: "goToLogin", sender: self)
            }
        })
    }
}
```
When logOut is completed, the closure's contents will be executed, unless if there was an error, then show an error to the user and skip removing the access token. 

I'm only trying to remove the access token and make sure it is empty before I perform the segue. 

So far this works for me, but then I'll have to call `AccessToken.refreshCurrentAccessToken` again in my login view to make sure access token is nil by the time I'm signed out. If I omitted that, my access token still contains the token. It is redudant to call `AccessToken.refreshCurrentAccessToken` again ni both views but so far this is the only thing that works for me now.
