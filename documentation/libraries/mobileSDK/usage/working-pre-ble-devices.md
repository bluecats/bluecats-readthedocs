---
layout: documentation
title: Working with Pre-BLE Devices
---

#### Android
The SDK will still work on lower versions of Android (sub 4.3), but Bluetooth Low Energy Scanning will be disabled. You will still be able to make use of the nearby sites functionality as this does not involve scanning for beacons.

#### iOS
While BLE iBeacon scanning is only available in iOS 7.0 and above, you may want to include the SDK in an app targeting both old and new devices. The following code can be used so that the SDK fails gracefully if the iOS version of the device is not supported. Note: This approach has been verified with a device running iOS 6.1.3.

Include a header import of CoreLocation in your AppDelegate:
##### Objective-C
```objc
#import <CoreLocation/CoreLocation.h>
```

##### Swift
```swift
import CoreLocation
```

Check for availability of CLBeaconRegion before starting purring:
##### Objective-C
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // Override point for customization after application launch.
    if ([CLBeaconRegion class]) {
        //Supports beacons
        [BlueCatsSDK startPurringWithAppToken:YourBCAppToken completion:^(BCStatus status) {
            //Complete initialisation of BlueCats SDK
        }];
    } else {
        //No beacon support
    }

    return YES;
}
```

##### Swift
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    // Override point for customization after application launch.

    if ((NSClassFromString("CoreLocationRegion")) != nil){
        BlueCatsSDK.startPurringWithAppToken("<#BLueCaTs-Apps-toKN-fRom-webDashBoarD#>", completion: { (BCStatus) -> Void in
            //Complete initialisation of BlueCats SDK
        })
    } else {
        //No beacon support
    }

   return true
}
```
