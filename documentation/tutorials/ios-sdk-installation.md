---
layout: documentation
title: iOS SDK Installation
category: Tutorials
order: 5
---

### Static Library Installation
The BlueCatsSDK static library is installed using CocoaPods. CocoaPods is an Objective-C library dependency manager. You can learn more about CocoaPods from [cocoapods.org](https://cocoapods.org/).

Add the BlueCatsSDK static library dependency to your Podfile in your Xcode project directory:  
```ruby
pod 'BlueCatsSDK', :git => 'https://github.com/bluecats/bluecats-ios-sdk.git'
```
Now you can install the BlueCatsSDK dependency in your project:  
```sh
$ pod install
```
To update the installed BlueCatsSDK dependency in your project, run the following command after ensuring `libBlueCatsSDK` is used:  
```sh
$ pod update
```

_Remember_, after installing the SDK into your project with cocoapods, open your project from the newly created `.xcworkspace` file.

#### Swift Bridging Header
To import the BlueCatsSDK's headers into a Swift project, add `#import "BlueCatsSDK.h"` to your bridging header.

#### Now you are ready to [startPurring](https://developer.bluecats.com/guides/startpurring#ios)!

### Requirements

**Important Note: iOS 11.0 introduces breaking changes on how location permission is requested for background 'Always' scanning**. See below for details on new NSLocationAlwaysAndWhenInUseUsageDescription plist key.

When using BlueCats SDK in an iOS app there are a number of static descriptions required to explain how the app will use Bluetooth and Beacon technology. These are used by iOS to customise permission prompts when requested by the app. These are included in the application plist file as explained below.

Required for App Store submission (not currently displayed to app users):

- [NSBluetoothPeripheralUsageDescription](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSBluetoothPeripheralUsageDescription) (Privacy - Bluetooth Peripheral Usage Description)

Location prompt to app users requesting permission to detect beacons either when the app is in use or also when it is closed:

Required when requesting **When in Use** permission only:

- for iOS 8.0 to iOS 11.0 support [NSLocationWhenInUseUsageDescription](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationWhenInUseUsageDescription)

or required when requesting **Always and/or When in Use** permission:

- for iOS 8.0 to iOS 10.0 support [NSLocationAlwaysUsageDescription](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationAlwaysUsageDescription)  and
- for iOS 11.0 support [NSLocationAlwaysAndWhenInUseUsageDescription](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization)

Additional Apple documentation on requesting location authorization including [escalating your app's authorization level from When in Use to Always](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization).

#### Privacy - Bluetooth Peripheral Usage Description in iOS 10.0

Starting in iOS 10 any app using CoreBluetooth is required to include a static 
[NSBluetoothPeripheralUsageDescription](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSBluetoothPeripheralUsageDescription) plist value in the application plist file (displayed as 'Privacy - Bluetooth Peripheral Usage Description'). This is a requirement for submission to the App Store for any app that includes the BlueCats SDK or other code that interacts with Bluetooth devices using the CoreBluetooth APIs.  The BlueCats SDK uses CoreBluetooth to monitor beacon battery life, apply remote secure settings updates, verify beacon ownership and detect secure beacons. Currently this description is not displayed to app users but could be used in the future so should include appropriate messaging about how beacons are being used by the app.

In case of a failed app submission, for example a rejection stating "Missing Info.plist key - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSBluetoothPeripheralUsageDescription key with a string value explaining to the user how the app uses this data." inclusion of the required key containing a message similar to that used for the location Always or While in Use descriptions should be sufficient to proceed with the submission process.

#### Location Access Description
Starting in iOS8, iOS requires a description to be presented to the user when an app requests access to a user’s location. It is also recommended that a description be provided when requesting access to a user's location on iOS7 devices.

Descriptions are provided by adding a Plist entry corresponding to the level of location use requested. Add a description for each level of location access you wish to request. These can easily be entered from the 'Info' section of your project target (see below) or directly into your Plist file.

Keys:
 - NSLocationWhenInUseUsageDescription
 - NSLocationAlwaysUsageDescription
 - NSLocationAlwaysAndWhenInUseUsageDescription

![NSLocationAlwaysUsageDescription](https://i.imgur.com/G55cRZi.png)

See the links below for more information on the different location access types:  
 - [NSLocationAlwaysAndWhenInUseUsageDescription](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization) 
 - [NSLocationAlwaysUsageDescription](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationAlwaysUsageDescription)  
 - [NSLocationWhenInUseUsageDescription](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationWhenInUseUsageDescription)
 - [NSLocationUsageDescription](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationUsageDescription).

#### Requesting Location Access
To request location use from the user, you can call:  
##### Objective-C
`[BlueCatsSDK requestAlwaysLocationAuthorization]`, or `[BlueCatsSDK requestWhenInUseLocationAuthorization]`.  
##### Swift  
` BlueCatsSDK.requestAlwaysLocationAuthorization()`, or `BlueCatsSDK.requestWhenInUseLocationAuthorization()`.

If the OS is pre-iOS8 `requestLocationAuthorization` will be called.

#### Ranging Secure Mode Beacons in the Background in iOS
In order to range secure beacons while an app in the background, you must select "Uses Bluetooth LE accessories" as a required background mode. This can be done easily from within Xcode. This is a setting that can be used to enable improved scanning capabilities of secure beacons while an app is in the background.

Note:
A demonstration of the functionality provided by this setting in the form of a video may be requested as part of the app store review submission process to explain why the background mode is required. An example may be a video recording demonstrating location based notifications from the app triggered by beacons.

![Uses bluetooth LE accessories checkbox ](https://s3-us-west-1.amazonaws.com/sdk-guide/gelato/usesLEAccessories.png)

For more information please see [UIBackground Modes](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW22).

The SDK limits total background scanning time to manage battery efficiency. These can be customised with the following [SDK Options](https://developer.bluecats.com/docs/versions/3.0/sdk-options#ios-specific):

`BCOptionBackgroundSessionTimeIntervalInSeconds`,  `BCOptionMaximumDailyBackgroundUsageInMinutes` and `BCScanInBackground`
