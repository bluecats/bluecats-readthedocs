Background
~~~~~~~~~~

Android 6.0 introduces a new permission model which changes the way
Location Services permissions are managed. This change breaks backwards
compatibility with versions of the BlueCats Android SDK version 1.13.6
or earlier when the app is compiled to target Android SDK Version 23:
http://android-developers.blogspot.com.au/2015/09/google-play-services-81-and-android-60.html

Android 6 disables by default the app’s permissions (specifically
Location Services permissions) if the app’s targetSdkVersion is 23. To
scan for beacons while the app is not active, the app will need to
display a dialog to the end user to choose Deny or Allow this app to
access the permission. It is the app developer’s responsibility to
request this permission by displaying the system request dialog at the
appropriate time.

Location services permission is required for BLE scanning on Android 6
and so is required for the BlueCats SDK to function. In previous
versions of Android, users had no ability to disable the location
services permission.

Impact
~~~~~~

If the app integrated with Bluecats SDK up to and including version
1.13.6 and is targeting to android SDK version 23 and the end user’s
device is running Android 6.0, the Bluecats SDK may encounter a fatal
crash if startPurring()/startPurringWithAppToken() is called before the
location permission is granted by user (i.e. if location permission is
not requested or while waiting for user selection of Deny/Allow).

For Apps Using BlueCats SDK version 1.13.6 or lower
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A workaround for apps using Bluecats SDK up to and including version
1.13.6 is to target the Android SDK 22 (Android 5.1 Lollipop) or lower.
This will allow support of Android end user devices up to and including
6.0 without experiencing this error.

Declare the permissions as usual in AndroidManifest.xml of your app, but
change

``<users-sdk android:minSdkVersion="18" android:targetSdkVersion="23" />``

to

``<users-sdk android:minSdkVersion="18" android:targetSdkVersion="22" />``

To enable scanning for beacons while the app is in a background state
the app will need to be updated to request the permission:

``Manifest.permission.ACCESS_FINE_LOCATION``

An example of requesting permissions in Android 6.0 is available here:
http://developer.android.com/training/permissions/requesting.html

For Apps Using BlueCats SDK version 1.13.7 or later
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BlueCats SDK version 1.13.7 add support for location permissions in apps
compiled to target Android SDK version 23. To enable scanning for
beacons while the app is in a background state the app will need to be
updated to request the permission:

``Manifest.permission.ACCESS_FINE_LOCATION``

An example of requesting permissions in Android 6.0 is available here:
http://developer.android.com/training/permissions/requesting.html
