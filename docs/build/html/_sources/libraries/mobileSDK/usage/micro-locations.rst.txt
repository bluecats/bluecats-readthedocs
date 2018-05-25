A ``BCMicroLocation`` object represents the sites and beacons in
proximity to the device. Observe ``BCMicroLocationManager``
notifications in iOS or implement ``BCMicrolocationDelegate`` /
``BCMicroLocationManagerCallback`` methods to handle beacon ranging
events.

For more information on using BCMicroLocations, please see our guide
`Using the
BCMicroLocationManager <https://developer.bluecats.com/guides/using-the-bcmicrolocationmanager>`__.

**Note: Micro-locations are deprecated in favor of BCTriggeredEvents and
BCBeaconManager in SDK versions > 2.0.0.**

Before you will receive micro-location updates, you must send
``startUpdatingMicroLocation()`` to your BCMicroLocationManager. To stop
receiving updates, call ``stopUpdatingMicroLocation()``.

Java
~~~~

.. code:: java

   BCMicroLocationManager.getInstance().startUpdatingMicroLocation( mMicroLocationManagerCallback );
   BCMicroLocationManager.getInstance().stopUpdatingMicroLocation( mMicroLocationManagerCallback );

   private BCMicroLocationManagerCallback mMicroLocationManagerCallback = new BCMicroLocationManagerCallback()
   {
       ...
   }

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   [[BCMicroLocationManager sharedManager] startUpdatingMicroLocation];
   [[BCMicroLocationManager sharedManager] stopUpdatingMicroLocation];

Swift
~~~~~

.. code:: swift

   BCMicroLocationManager.sharedManager().startUpdatingMicroLocation()
   BCMicroLocationManager.sharedManager().stopUpdatingMicroLocation()

Get notified when nearby sites are updated
------------------------------------------

.. _java-1:

Java
~~~~

.. code:: java

   @Override
   public void onDidUpdateNearbySites( final List<BCSite> list )
   {
       ...
   }

.. _objective-c-1:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateNearbySites:(NSArray *)sites {
     ...
   }

.. _swift-1:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateNearbySites sites: [AnyObject]!) {
     ...
   }

Get notified when the device enters a site
------------------------------------------

.. _java-2:

Java
~~~~

.. code:: java

   @Override
   public void onDidEnterSite( final BCSite bcSite )
   {
       ...
   }

.. _objective-c-2:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didEnterSite:(BCSite *)site {
     ...
   }

.. _swift-2:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didEnterSite site: BCSite!) {
     ...
   }

Get notified when the device exits a site
-----------------------------------------

.. _java-3:

Java
~~~~

.. code:: java

   @Override
   public void onDidExitSite( final BCSite bcSite )
   {
       ...
   }

.. _objective-c-3:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didExitSite:(BCSite *)site {
     ...
   }

.. _swift-3:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didExitSite site: BCSite!) {
     ...
   }

Get notified when the device ranges beacons
-------------------------------------------

.. _java-4:

Java
~~~~

.. code:: java

   @Override
   public void onDidRangeBeaconsForSiteID( final BCSite bcSite, final List<BCBeacon> list )
   {
       ...
   }

.. _objective-c-4:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didRangeBeacons:(NSArray *)beacons inSite:(BCSite *)site {
     ...
   }

.. _swift-4:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didRangeBeacons beacons: [AnyObject]!, inSite site: BCSite!) {
     ...
   }

Get notified when the a new micro-location is available
-------------------------------------------------------

.. _java-5:

Java
~~~~

.. code:: java

   @Override
   public void onDidUpdateMicroLocation( final List<BCMicroLocation> list )
   {
       ...
   }

.. _objective-c-5:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateMicroLocations:(NSArray *)microLocations {
     ...
   }

.. _swift-5:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateMicroLocations microLocations: [AnyObject]!) {
     ...
   }

Get notified when micro-location services availability is updated (iOS only)
----------------------------------------------------------------------------

.. _objective-c-6:

Objective-C
~~~~~~~~~~~

.. code:: objectivec

   - (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateMicroLocationServicesAvailibility:(BOOL)available {
     ...
   }

.. _swift-6:

Swift
~~~~~

.. code:: swift

   func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateMicroLocationServicesAvailibility available: Bool) {
     ...
   }
