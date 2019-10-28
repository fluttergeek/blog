---
title: Using Touch ID and Face ID In Your App (Swift 5)
date: 2019-10-28 20:00:00 Z
categories:
- Tutorial
tags:
- biometric
- touch id
- face id
- swift
image: assets/images/iphone-x-faceid-670x335.jpg

---
To use Face ID, or even Touch ID, we need to import `LocalAuthentication`. The code below works for both Face and Touch ID. Whichever is enabled in the user's settings.

    let context = LAContext()
    var error: NSError?
    
    if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) {
    	// This reason is only for Touch ID, not Face ID
        // The reason for accessing Face ID, is found in Info.plist
        let reason: String = "Please use Touch ID to unlock"
        
        context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: reason) { [weak self] success, authenticationError in
        	Dispatch.main.async {
            	if success {
                	// do some unlocking of data or
                    // present new view
                } else {
                	// Face wasn't detected
                    // Maybe user was wearing sunglasses
                    if error = error {
                      let ac = UIAlertController(title: "Authetication Failed!"),
                                  message: self.reportTouchIDError(error: error),
                                  preferredStyle: .alert)
                      ac.addAction(UIAlertAction(title: "OK", style: .default))
                      self?.present(ac, animated: true)
                    }
                }
            }
        }
    } else {
    	// Biometric is not supported by your iOS device
        let ac = UIAlertController(title: "Biometric Authentication Unavailable"),
                      message: "Your device is not configured for biometric authentication.",
                      preferredStyle: .alert)
        ac.addAction(UIAlertAction(title: "OK", style: .default))
        self?.present(ac, animated: true)
    }

Before this can work, we must add a key value in our Info.plist to alert the user that we are going to be using this biometric feature.

Key: Privacy - Face ID Usage Description

Type: String

Value: This app uses Face ID to confirm your identity

There's a more specific way of producing an error. This example was explained by `David Tran`:

    func reportTouchIDError(error: NSError) -> String {
    	switch error.code {
        case LAError.AuthenticationFailed.rawValue:
        	return "Authentication Failed!"
        case LAError.PasscodeNotSet.rawValue:
        	return "Passcode not set!"
        case LAError.SystemCancel.rawValue:
        	return "Authentication was cancelled by the system!"
        case LAError.UserCancel.rawValue:
        	return "User cancel auth"
        case LAError.TouchIDNotEnrolled.rawValue:
        	return "User has not enrolled any finger with touch ID"
        case LAError.TouchIDNotAvailable.rawValue:
        	return "Touch ID is not available"
        case LAError.UserFallBack.rawValue:
        	return "User tapped enter password"
        default:
        	return error.localizedDescription
        }
    }

<iframe width="560" height="315" src="https://www.youtube.com/embed/vf7JyFFSNDU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>