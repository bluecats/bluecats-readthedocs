The ``BCZone`` class defines an object that represents a dynamic
micro-location. A ``BCZone`` can be composed of a site, a category,
multiple categories and specific beacons. Zones can also be configured
to trigger dwell time events.

Initializing the Zone Monitor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Custom values placed on beacons, categories and sites define which sites
are included within a zone. You must pass the key for these custom
values to the zone monitor upon initialization.

Java
''''

.. code:: java

   @Override
   protected void onCreate( final Bundle savedInstanceState )
   {
       ...
       final List<String> zoneIdentifierKeys = Arrays.asList( "ZONE_ID_KEY" );
       final BCZoneMonitor zoneMonitor = new BCZoneMonitor( mZoneMonitorCallback, zoneIdentifierKeys );
       zoneMonitor.startMonitoringZones();
   }

   private final BCZoneMonitorCallback mZoneMonitorCallback = new BCZoneMonitorCallback()
   {
       ...
   }

Objective-C
'''''''''''

.. code:: objectivec

   NSArray *zoneIdentifierKeys = @[@"ZONE_ID_KEY";
   _zoneMonitor = [[BCZoneMonitor alloc] initWithDelegate:self queue:nil zoneIdentifierKeys:keys];
   [_zoneMonitor startMonitoringZones];

Swift
'''''

.. code:: swift

   let zoneIdentifierKeys = ["ZONE_ID_KEY"]
   zoneMonitor = BCZoneMonitor.init(delegate: self, queue: nil, zoneIdentifierKeys: zoneIdentifierKeys)
   zoneMonitor.startMonitoringZones()

Get notified when the device enters a site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-1:

Java
''''

.. code:: java

   @Override
   public void didEnterSite( final BCSite site )
   {
       ...
   }

.. _objective-c-1:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didEnterSite:(BCSite *)site {
       ...
   }

.. _swift-1:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didEnterSite site: BCSite!) {
       ...
   }

Get notified when the device enters a zone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-2:

Java
''''

.. code:: java

   @Override
   public void didEnterZone( final BCZone zone )
   {
       ...
   }

.. _objective-c-2:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didEnterZone:(BCZone *)zone {
       ...
   }

.. _swift-2:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didEnterZone zone: BCZone!) {
       ...
   }

Get notified when the device re-enters a zone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-3:

Java
''''

.. code:: java

   @Override
   public void didReenterZone( final BCZone zone )
   {
       ...
   }

.. _objective-c-3:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didReEnterZone:(BCZone *)zone {
       ...
   }

.. _swift-3:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didReEnterZone zone: BCZone!) {
       ...
   }

Get notified when the device ranges beacons in a zone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-4:

Java
''''

.. code:: java

   @Override
   public void didRangeBeacons( final List<BCBeacon> beacons, final BCZone inZone )
   {
       ...
   }

.. _objective-c-4:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didRangeBeacons:(NSArray *)beacons inZone:(BCZone *)zone {
       ...
   }

.. _swift-4:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didRangeBeacons beacons: [AnyObject]!, inZone zone: BCZone!) {
       ...
   }

Get notified when your dwell time interval is met
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Dwell time events occur when the device dwells in the specified zone
matching the dwell time interval. ##### Java

.. code:: java

   @Override
   public void didDwellInZone( final BCZone zone, final Integer dwellTimeInterval )
   {
       ...
   }

.. _objective-c-5:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didDwellInZone:(BCZone *)zone forTimeInterval:(NSTimeInterval)dwellTimeInterval {
       ...
   }

.. _swift-5:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didDwellInZone zone: BCZone!, forTimeInterval dwellTimeInterval: NSTimeInterval) {
       ...
   }

Get notified when the device exits a zone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-5:

Java
''''

.. code:: java

   @Override
   public void didExitZone( final BCZone zone )
   {
       ...
   }

.. _objective-c-6:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor didExitZone:(BCZone *)zone {
       ...
   }

.. _swift-6:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, didExitZone zone: BCZone!) {
       ...
   }

Get notified when zone monitoring is suspended
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-6:

Java
''''

.. code:: java

   @Override
   public void willSuspendMonitoringInSite( final BCSite site, final Date untilDate )
   {
       ...
   }

.. _objective-c-7:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor willSuspendMonitoringInSite:(BCSite *)site untilDate:(NSDate *)date {
       ...
   }

.. _swift-7:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, willSuspendMonitoringInSite site: BCSite!, untilDate date: NSDate!) {
       ...
   }

Get notified when zone monitoring will resume
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-7:

Java
''''

.. code:: java

   @Override
   public void willResumeMonitoringInSite( final BCSite site )
   {
       ...
   }

.. _objective-c-8:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)zoneMonitor:(BCZoneMonitor *)monitor willResumeMonitoringInSite:(BCSite *)site {
       ...
   }

.. _swift-8:

Swift
'''''

.. code:: swift

   func zoneMonitor(monitor: BCZoneMonitor!, willResumeMonitoringInSite site: BCSite!) {
       ...
   }
