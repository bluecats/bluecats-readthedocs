---
layout: documentation
title: Android SDK Installation
category: Tutorials
order: 6
---

[![Download](https://bintray.com/package/binaryButton?versionPath=%2Fbluecats%2Fmaven%2Fbluecats-android-sdk%2F2.0.4&version=2.0.4)](https://bintray.com/bluecats/maven/bluecats-android-sdk/_latestVersion)

### Step 1 - Installation
#### a) Using Android Studio
Add the following to your build.gradle:
```gradle
compile 'com.bluecats:bluecats-android-sdk:2.0.4'
```

Add the following to your proguard rules:
```
-keep class com.bluecats.sdk.** {*;}
```

#### b) Using Eclipse

* Download .aar file from [bintray](https://bintray.com/bluecats/maven/bluecats-android-sdk/_latestVersion), you will get an .aar file: bluecats-android-sdk-${version}.aar 
* Change the extension to .zip from .aar, and unzip it into bluecats-android-sdk folder, for example.
* In the folder bluecats-android-sdk, you will see a .jar file: classes.jar, change the name to a meaningful name, like bluecats-android-sdk.jar, copy it to your Eclipse project's "libs" folder.
* In the folder bluecats-android-sdk/jni, you will see several ABI folders, copy all the folders into your Eclipse project's "libs" folder.
* Your "libs" folder of eclipse project looks like this:
```
libs/
    bluecats-android-sdk.jar
    armebi/
          libbluecats_sdk.so
    armebi-v7a/
              libbluecats_sdk.so
    ...
    ...
    ...
```

* Add bluecats-android-sdk.jar to your build path of eclipse project.
* Merge manually the AndroidManifest.xml into your own AndroidManifest.xml to include the permission and service declarations.
* add necessary 3rd party libraries: Gson, android Support

### Step 2 - Getting an App Token
Get an app token from [app.bluecats.com/apps](https://app.bluecats.com/apps) by clicking "Create New App" and giving your new app a name. Once created, your new app should appear in the list and you'll be able to access your app token by clicking the "Show App Token / Secret" button below it.

NOTE: The login used for the developer portal is not the same login used for app.bluecats.com. If you don't already have an account on [app.bluecats.com](https://app.bluecats.com/), you will need to create one in order to progress past this point. If you're unsure of what to do, send an email to [sales@bluecats.com](mailto:sales@bluecats.com) and we'll gladly help you out!

### Step 3 - startPurring
Start purring!
```java
BlueCatsSDK.startPurringWithAppToken( getApplicationContext(), "YOUR_APP_TOKEN" );
```

**More tips and tweaks [here](https://developer.bluecats.com/guides/startpurring#android)!**

### Requirements
#### AndroidManifest.xml
A number of permissions and dependencies are added automatically by the SDK, and can be found [here](https://developer.bluecats.com/guides/android-migrating-from-1-13-8-to-2-0-0-2-0-1).

If you want the SDK to start when the device starts, you may add this to your xml:
```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

<application ...>
	...

	<receiver android:name="com.bluecats.sdk.BlueCatsSDKServiceReceiver">
	    <intent-filter>
	        <action android:name="android.intent.action.BOOT_COMPLETED"/>
	    </intent-filter>
	</receiver>
</application>
```

#### Android 6.0+ (Marshmallow) Location Permissions
Android 6.0 introduces a new permission model which changes the way Location Services permissions are managed. Before scanning for beacons the app is required to request location permissions from the user via a system dialogue. Detailed information on implementing location permission support in Android 6.0 can be found [here](https://developer.bluecats.com/guides/android-6-0-and-location-services-permissions).

#### Android SDK 4.3+ (API Level 18+)
The SDK will still work on lower versions of Android down to 2.3 (API Level 9), but Bluetooth Low Energy Scanning will be disabled. You will still be able to make use of the Sites Nearby functionality as this does not involve scanning for beacons.

### Any Questions?
See the sample [BlueCats Scratching Post](https://github.com/bluecats/bluecats-scratchingpost-android) app for usage and integration instructions.
