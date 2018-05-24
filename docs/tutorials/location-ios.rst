Starting in iOS8, iOS requires a description for why the app accesses
the user’s location information to be provided to the user when
requesting location access. It is greatly recommend that a description
be provided when requesting access to a user’s location on iOS7 devices
as well.

Descriptions are provided by adding Plist entries corresponding to the
levels of location use requested. Add a key value pair for each level of
location permission you wish to request with the key representing the
level of location access and the value of the description. These can
easily be entered from the Info section of your project target (see
below) or directly into your Plist file.

| Keys: - NSLocationAlwaysUsageDescription
| - NSLocationWhenInUseUsageDescription
| - NSLocationUsageDescription

.. figure:: http://i.imgur.com/G55cRZi.png
   :alt: NSLocationAlwaysUsageDescription

   NSLocationAlwaysUsageDescription

Please see
`NSLocationAlwaysUsageDescription <https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationAlwaysUsageDescription>`__,
`NSLocationWhenInUseUsageDescription <https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationWhenInUseUsageDescription>`__,
and
`NSLocationUsageDescription <https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSLocationUsageDescription>`__.

Requesting Location Access
^^^^^^^^^^^^^^^^^^^^^^^^^^

| To request location use from the user, call:
| ``[BlueCatsSDK requestAlwaysLocationAuthorization]``,
| ``[BlueCatsSDK requestWhenInUseLocationAuthorization]``, or
| ``[BlueCatsSDK requestLocationAuthorization]`` on iOS7 devices.
