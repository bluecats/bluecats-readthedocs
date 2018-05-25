To ensure beacons are discovered at the right time, there are a number
of options available to customise how beacon metadata is retrieved and
cached in your app. See `SDK
Options <https://developer.bluecats.com/reference/docs/sdk-options>`__
for full details on SDK settings. Use caution when using options that
cache all beacons in large and/or closely installed networks as this may
adversely affect the performance of your app.

1. Only load beacons as they are discovered or in nearby locations by geo-fence (default behaviour):
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Java
''''

.. code:: java

   BC_OPTION_CACHE_ALL_BEACONS_FOR_APP: "false"
   BC_OPTION_DISCOVER_BEACONS_NEARBY: "true"

Objective-C
'''''''''''

.. code:: objc

   BCOptionCacheAllBeaconsForApp:NO
   BCOptionDiscoverBeaconsNearby:YES

Swift
'''''

.. code:: swift

   BCOptionCacheAllBeaconsForApp: false
   BCOptionDiscoverBeaconsNearby: true

2. Load all beacons and sites for the app and ignore all other beacons:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-1:

Java
''''

.. code:: java

   BC_OPTION_CACHE_ALL_BEACONS_FOR_APP: "true"
   BC_OPTION_DISCOVER_BEACONS_NEARBY: "false"

.. _objective-c-1:

Objective-C
'''''''''''

.. code:: objc

   BCOptionCacheAllBeaconsForApp:YES
   BCOptionDiscoverBeaconsNearby:NO

.. _swift-1:

Swift
'''''

.. code:: swift

   BCOptionCacheAllBeaconsForApp: true
   BCOptionDiscoverBeaconsNearby: false

3. Load all beacons for app first, then discover other public beacons nearby:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-2:

Java
''''

.. code:: java

   BC_OPTION_CACHE_ALL_BEACONS_FOR_APP: "true"
   BC_OPTION_DISCOVER_BEACONS_NEARBY: "true"

.. _objective-c-2:

Objective-C
'''''''''''

.. code:: objc

   BCOptionCacheAllBeaconsForApp:YES
   BCOptionDiscoverBeaconsNearby:YES

.. _swift-2:

Swift
'''''

.. code:: swift

   BCOptionCacheAllBeaconsForApp: true
   BCOptionDiscoverBeaconsNearby: true

4. Don’t sync any discovered beacons:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-3:

Java
''''

.. code:: java

   BC_OPTION_CACHE_ALL_BEACONS_FOR_APP: "false"
   BC_OPTION_DISCOVER_BEACONS_NEARBY: "false"

.. _objective-c-3:

Objective-C
'''''''''''

.. code:: objc

   BCOptionCacheAllBeaconsForApp:NO
   BCOptionDiscoverBeaconsNearby:NO

.. _swift-3:

Swift
'''''

.. code:: swift

   BCOptionCacheAllBeaconsForApp: false
   BCOptionDiscoverBeaconsNearby: false
