---
layout: documentation
title: Getting Started with Swift
category: Tutorials
order: 9
---

Lets start using the Beacons with Swift using the BlueCats SDK. 

## Installing CocoaPods

The BlueCatsSDK static library is installed using CocoaPods. [CocoaPods](https://cocoapods.org/) is an Objective-C library dependency manager. 


1. Open your terminal
2. Install CocoaPods using the commands below

	```bash
	# Xcode 7 + 8  
	sudo gem install cocoapods
	```

3. Once your CocoaPods are installed, create your XCode project. We're going to call ours `myBCSwiftProject` and save it to our desktop
4. Close out Xcode
5. Change directories to your new project and initialize your pod

	```bash
	cd ~/Desktop/myBCSwiftProject
	pod init
	```

6. Open the `Podfile` with your favorite text editor. Add our pod dependencies, and specify your app as the new target 

	```bash
	platform :ios, '8.0'
	use_frameworks!
	
	target 'myBCSwiftProject' do
		pod 'BlueCatsSDK', :git => 'https://github.com/bluecats/bluecats-ios-sdk.git'
	end
	```
7. Save and close the file
8. Reopen your terminal install and update your dependencies 

	```bash
	pod install
	```

9.  Once your dependencies have been installed. You need to open the Xcode workspace rather than the project file when building your project

	```bash
	open myBCSwiftProject.xcworkspace
	```
You can close the terminal. The rest of the tutorial will only require using Xcode


## Creating the Objective-C Bridging Header
1. Open up your Xcode Project, and create a new Objective-C file (it doesn't matter what we name it because we are deleting it shortly)
2. You will be prefaced asking if you want to create an Objective-C Bridging Header. Click "Create Bridging Header"

  ![Bridging-Header](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/GettingStartedWithSwift/Bridging-Header.png)
  
3. Delete your Objective-C file 
4. Insert the line `#import "BlueCatsSDK.h"` into your newly formed header file 

## Requirements and Privacy Settings
When using BlueCats SDK in an iOS app there are a number of static descriptions required to explain how the app will use Bluetooth and Beacon technology. These are used by iOS to customize permission prompts when requested by the app. These are included in the application plist file as explained below. For more information click [here](https://developer.bluecats.com/guides/ios-sdk-installation-c99b3b62-a271-4321-9c29-ce61699fce4f#requirements).

We will be using these two properties: 

* [Bluetooth Peripheral Usage Description](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSBluetoothPeripheralUsageDescription):
This key lets you describe the reason your app uses Bluetooth. When the system prompts the user to allow usage, the value that you provide for this key is displayed as part of the alert.
 
* [Location When In Use Usage Description](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationWhenInUseUsageDescription): This key lets you describe the reason your app accesses the user’s location information while your app runs in the foreground and otherwise when in use. Include this key when your app uses location services to track the user’s current location directly.

1. Go to your project files and open `Info.plist` 
2. Under Information Property List, add the keys with the values:
 
	**Privacy - Bluetooth Peripheral Usage Description**
	We use Bluetooth to transfer data to beacons to update settings or firmware.
	
	**Privacy - Location When In Use Usage Description** 
	We use your location to provide information based on your location in store.

	![Swift-Plist](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/GettingStartedWithSwift/Swift-Plist.png)

	Lastly, we need to enable the Bluetooth LE Background mode.

4. Go to the `Capabilities` Tab 
5. Turn on Background Modes
6. Then check `Uses Bluetooth LE accessories`

![uses-BLE-acessories](https://s3-us-west-1.amazonaws.com/sdk-guide/gelato/usesLEAccessories.png)

For more information please see [UIBackground Modes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW22).

The SDK limits total background scanning time to manage battery efficiency. These can be customized with the following [SDK Options](https://developer.bluecats.com/docs/versions/3.0/sdk-options#ios-specific):

## Adding your Swift Code
To begin ranging your beacons, call the startPurring or startPurringWithAppToken methods of the BlueCatsSDK. App tokens can be obtained from the apps section of the [BlueCats web app](https://app.bluecats.com/apps). The login used for app.bluecats.com is not the same login used to access our developer portal. If you did not receive a login with your beacons contact support@bluecats.com.

Go to your project files and paste this code into `AppDelegate.swift`. Replace the first func application, so it looks like the code below. 

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        BlueCatsSDK.startPurring(withAppToken: "BLueCaTs-Apps-toKN-fRom-webDashBoarD", completion: { (BCStatus) -> Void in
            let appTokenVerificationStatus: BCAppTokenVerificationStatus = BlueCatsSDK.appTokenVerificationStatus()
            if (appTokenVerificationStatus == .notProvided || appTokenVerificationStatus == .invalid){
            }
            if (!BlueCatsSDK.isLocationAuthorized()){
                BlueCatsSDK.requestWhenInUseLocationAuthorization()
            }
            if (!BlueCatsSDK.isNetworkReachable()){
            }
            if (!BlueCatsSDK.isBluetoothEnabled()){
                //Prompt user to enable bluetooth in settings. If BLE is required for current functionality a modal is recommended.
                
            }
        })
        
        return true
    }
```

Once you add your code, paste your App token into `BLueCaTs-Apps-toKN-fRom-webDashBoarD` from the [BlueCats web app](https://app.bluecats.com/apps)

##Some Visual Feedback

We are *technically* done, but I like visual feedback.
Let's add some code so that we can see the serial numbers in our terminal. 

Paste this code into the ViewController:

```swift
class ViewController: UIViewController, BCBeaconManagerDelegate{
	
    var beaconManager: BCBeaconManager?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        beaconManager = BCBeaconManager()
        beaconManager!.delegate = self
    }
	
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    func beaconManager(_ monitor: BCBeaconManager!, didRangeBeacons beacons: [BCBeacon]!){
        for currentBeacon in beacons! {
            if let serial = currentBeacon.serialNumber{
                print ("Beacon serial number is \(serial)")
            }
        }
    
    }
	
}
```



The BCBeaconManager allows you to search for all of your beacons. 
Unfortunately, Xcode's simulator does not allow you to test Bluetooth applications, so we must load it to an actual iOS device. 

* Connect your iOS device and select it as a target. 
Once you run the code on it. 
You may be prompted to go into Settings > Device Management > your@email.com > Click "Trust your@email.com"

* Open your terminal, and you should see some output similar to this. 
![Swift-Serial-Output](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/GettingStartedWithSwift/Swift-Serial-Output.png)

If you do, congratulations! 
You are now ready to start programming your BlueCats Swift App!


