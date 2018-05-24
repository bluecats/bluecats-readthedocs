---
layout: documentation
title: Start Purring
category: Tutorials
order: 1 
---

## Ready to `startPurring`?
To begin ranging your beacons, call the startPurring or startPurringWithAppToken methods of the BlueCatsSDK. App tokens can be obtained from the apps section of the [BlueCats web app](https://app.bluecats.com/apps). The login used for app.bluecats.com is not the same login used to access our developer portal. If you did not receive a login with your beacons contact support@bluecats.com.

### Android  
Fire up the BlueCats SDK in your main activity's onCreate() method:
```java
BlueCatsSDK.startPurringWithAppToken( getApplicationContent(), "YOUR_APP_TOKEN" );
```

Verification status can be checked using the following code snippets:
```java
final BlueCatsSDK.BCAppTokenVerificationStatus appTokenVerificationStatus = BlueCatsSDK.getAppTokenVerificationStatus();
if( appTokenVerificationStatus == BlueCatsSDK.BCAppTokenVerificationStatus.BC_APP_TOKEN_VERIFICATION_STATUS_NOT_PROVIDED )
{
	//The app token hasn't been provided; do something.
}

if( !BlueCatsSDK.isLocationAuthorized() )
{
	//No GPS available; enable GPS.
}

if( !BlueCatsSDK.isNetworkReachable() )
{
	//No network reachable; enable network connection.
}

if( !BlueCatsSDK.isBluetoothEnabled() )
{
	//Bluetooth is turned off; enable it.
}
```

To ensure that the SDK knows about your app state, `didEnterBackground()` and `didEnterForeground()` should be called when the app pauses and resumes, like below:
```java
@Override
protected void onPause()
{
	super.onPause();

	BlueCatsSDK.didEnterBackground();
}

@Override
protected void onResume()
{
	super.onResume();

	BlueCatsSDK.didEnterForeground();
}
```

You can also enable the SDK Service to run from boot by adding the following to your AndroidManifest.xml:
```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

<application ...>
	...

	<receiver android:name="com.bluecats.sdk.BlueCatsSDKServiceReceiver" >
	    <intent-filter>
	        <action android:name="android.intent.action.BOOT_COMPLETED" />
	    </intent-filter>
	</receiver>
</application>
```

### iOS  
##### Objective-C
```objective-c
[BlueCatsSDK startPurringWithAppToken:@"<#BLueCaTs-Apps-toKN-fRom-webDashBoarD#>" completion:^(BCStatus status) {
    if (status == kBCStatusPurringWithErrors) {
        BCAppTokenVerificationStatus appTokenVerificationStatus = [BlueCatsSDK appTokenVerificationStatus];
        if (appTokenVerificationStatus == kBCAppTokenVerificationStatusNotProvided || appTokenVerificationStatus == kBCAppTokenVerificationStatusInvalid) {
            //kBCAppTokenVerificationStatusNotProvided - Use setAppToken to set the app token. Get an app token from app.bluecats.com
            //kBCAppTokenVerificationStatusInvalid - App token invalid.
        }
        if (![BlueCatsSDK isLocationAuthorized]) {
            [BlueCatsSDK requestAlwaysLocationAuthorization]; //Requests location use from the user even when the app is not in use.
            //[BlueCatsSDK requestWhenInUseLocationAuthorization]; //Requests location use when the app is in use.
        }
        if (![BlueCatsSDK isNetworkReachable]) {
            //The BlueCats SDK must have network connectivity at least once before ranging beacons. If this is the only error and the SDK has never reached the network purring will occur with network connectivity.
        }
        if (![BlueCatsSDK isBluetoothEnabled]) {
            //Prompt user to enable bluetooth in settings. If BLE is required for current functionality a modal is recommended.
        }
    }
}];
```

##### Swift
```swift
BlueCatsSDK.startPurringWithAppToken("<#BLueCaTs-Apps-toKN-fRom-webDashBoarD#>", completion: { (BCStatus) -> Void in
    let appTokenVerificationStatus: BCAppTokenVerificationStatus = BlueCatsSDK.appTokenVerificationStatus()
    if (appTokenVerificationStatus == .NotProvided || appTokenVerificationStatus == .Invalid){
        //NotProvided - Use setAppToken to set the app token. Get an app token from app.bluecats.com/apps
        //Invalid - App token invalid.
    }
    if (!BlueCatsSDK.isLocationAuthorized()){
        BlueCatsSDK.requestAlwaysLocationAuthorization() //Requests location use from the user even when the app is not in use.
        //BlueCatsSDK.requestWhenInUseLocationAuthorization() //Requests location use when the app is in use.
    }
    if (!BlueCatsSDK.isNetworkReachable()){
        //The BlueCats SDK must have network connectivity at least once before ranging beacons. If this is the only error and the SDK has never reached the network purring will occur with network connectivity.
    }
    if (!BlueCatsSDK.isBluetoothEnabled()){
       //Prompt user to enable bluetooth in settings. If BLE is required for current functionality a modal is recommended.
    }
})
```

### Now that you've `startedPurring`, check out our [tutorials](https://developer.bluecats.com/guides/categories/tutorials).
