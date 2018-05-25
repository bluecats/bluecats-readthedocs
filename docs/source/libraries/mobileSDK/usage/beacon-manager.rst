The beacon manager is the central point for configuring the delivery of
beacon ranging events to your app. Use this class if you wish to create
reactions to individual beacons or beacons in particular modes.

If you haven’t installed our SDK, head over to the installation pages
for `Android <https://github.com/bluecats/bluecats-android-sdk>`__ or
`iOS <https://github.com/bluecats/bluecats-ios-sdk>`__.

**Note: The beacon manager is only available in SDK versions > 2.0.0.**

Once purring, the BlueCats SDK begins scanning for all beacons the app
has been granted access to. If you wish to scan for beacons outside of
the BlueCats platform, create new BCBeaconRegions and use the
``startMonitoringBeaconRegion`` method.

Beacon Manager Set-up
^^^^^^^^^^^^^^^^^^^^^

Java
''''

.. code:: java

   @Override
   protected void onCreate( final Bundle savedInstanceState )
   {
       ...
       //Pass in a callback to the SDK to call for the relevant events in BCBeaconManagerCallback.
       //Each beacon manager can only have one callback
       final BCBeaconManager beaconManager = new BCBeaconManager();
       beaconManager.registerCallback( mBeaconManagerCallback );
   }

   private final BCBeaconManagerCallback mBeaconManagerCallback = new BCBeaconManagerCallback()
   {
       ...
   }

Objective-C
'''''''''''

.. code:: objectivec

   #import "BlueCatsSDK/BlueCatsSDK.h" //import the main header or individual headers

   @interface ViewController () <BCBeaconManagerDelegate>
   @property (strong, nonatomic) BCBeaconManager *beaconManager;
   @end

   @implementation ViewController

   - (void)viewDidLoad {
       [super viewDidLoad];

       self.beaconManager = [[BCBeaconManager alloc] initWithDelegate:self queue:nil];
   }

Swift
'''''

.. code:: swift

   import BlueCatsSDK //if using the framework, otherwise this is unneeded

   class ViewController: UIViewController, BCBeaconManagerDelegate {

       var beaconManager = BCBeaconManager()

       override func viewDidLoad() {
           super.viewDidLoad()

           beaconManager = BCBeaconManager.init(delegate: self, queue: nil)
       }

Get notified when the device comes into the range of beacons
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-1:

Java
''''

.. code:: java

   @Override
   public void didEnterBeacons( final List<BCBeacon> beacons )
   {
       Log.d( TAG, "didEnterBeacons: " + beacons.size() );
   }

.. _objective-c-1:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didEnterBeacons:(NSArray<BCBeacon *> *)beacons {
     NSLog(@"didEnterBeacons: %lu", (unsigned long)[beacons count]);
   }

.. _swift-1:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didEnterBeacons beacons: [BCBeacon]!) {
       print("didEnterBeacons: \(beacons.count)")
   }

Get notified when new beacon ranging information is available
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-2:

Java
''''

.. code:: java

   @Override
   public void didRangeBeacons( final List<BCBeacon> beacons )
   {
       Log.d( TAG, "didRangeBeacons: " + beacons.size() );
   }

.. _objective-c-2:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didRangeBeacons:(NSArray<BCBeacon *> *)beacons {
     NSLog(@"didRangeBeacons: %lu", (unsigned long)[beacons count]);
   }

.. _swift-2:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didRangeBeacons beacons: [BCBeacon]!) {
       print("didRangeBeacons: \(beacons.count)")
   }

Get notified when the device exits the range of beacons
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-3:

Java
''''

.. code:: java

   @Override
   public void didExitBeacons( final List<BCBeacon> beacons )
   {
       Log.d( TAG, "didExitBeacons: " + beacons.size() );
   }

.. _objective-c-3:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didExitBeacons:(NSArray<BCBeacon *> *)beacons {
     NSLog(@"didExitBeacons: %lu", (unsigned long)[beacons count]);
   }

.. _swift-3:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didExitBeacons beacons: [BCBeacon]!) {
       print("didExitBeacons: \(beacons.count)")
   }

Get notified when the device enters a site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-4:

Java
''''

.. code:: java

   @Override
   public void didEnterSite( final BCSite site )
   {
       Log.d( TAG, "didEnterSite: " + site.getName() );
   }

.. _objective-c-4:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didEnterSite:(BCSite *)site {
     NSLog(@"didExitBeacons: %lu", (unsigned long)[beacons count]);
   }

.. _swift-4:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didEnterSite site: [BCSite]!) {
       print("didEnterSite: \(beacons.count)")
   }

Get notified when the device exits a site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-5:

Java
''''

.. code:: java

   @Override
   public void didExitSite( final BCSite site )
   {
       Log.d( TAG, "didExitSite: " + site.getName() );
   }

.. _objective-c-5:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didExitSite:(BCSite *)site {
     NSLog(@"didExitSite: %lu", (unsigned long)[beacons count]);
   }

.. _swift-5:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didExitSite site: [BCSite]!) {
       print("didExitSite: \(beacons.count)")
   }

Get notified when the SDK ranges an Eddystone beacon
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-6:

Java
''''

.. code:: java

   @Override
   public void didRangeEddystoneBeacons( final List<BCBeacon> eddystoneBeacons )
   {
       Log.d( TAG, "didRangeEddystoneBeacons: " + eddystoneBeacons.size() );
   }

.. _objective-c-6:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didRangeEddystoneBeacons:(NSArray<BCBeacon *> *)beacons {
     NSLog(@"didExitSite: %lu", (unsigned long)[beacons count]);
   }

.. _swift-6:

swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didRangeEddystoneBeacons beacons: [BCBeacon]!) {
     print("didRangeEddystoneBeacons: \(beacons.count)")
   }

Get notified when the SDK discovers an Eddystone URL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _java-7:

Java
''''

.. code:: java

   @Override
   public void didDiscoverEddystoneURL( final URL eddystoneUrl )
   {
       Log.d( TAG, "didDiscoverEddystoneURL: " + eddystoneUrl );
   }

.. _objective-c-7:

Objective-C
'''''''''''

.. code:: objectivec

   - (void)beaconManager:(BCBeaconManager *)beaconManager didDiscoverEddystoneURL:(NSURL *)eddystoneURL {
     NSLog(@"didDiscoverEddystoneURL: %@", [eddystoneURL absoluteString]);
   }

.. _swift-7:

Swift
'''''

.. code:: swift

   func beaconManager(beaconManager: BCBeaconManager!, didDiscoverEddystoneURL eddystoneURL: NSURL!) {
     print("didDiscoverEddystoneURL: \(eddystoneURL.absoluteString)")
   }
