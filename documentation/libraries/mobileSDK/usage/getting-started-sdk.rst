The BlueCats platform includes everything you need to add spatial
intelligence to your application and manage beacons at scale. Our goal
is to enable smart devices to achieve spatial intelligence with the
least amount of development. Install our SDKs for
`Android <https://developer.bluecats.com/guides/android-sdk-installation-48c4e341-ecd4-43f3-8f37-581e43fc53ab>`__
and
`iOS <https://developer.bluecats.com/guides/ios-sdk-installation-c99b3b62-a271-4321-9c29-ce61699fce4f>`__
with just a few lines of code and remote configuration, beacon visit
logging and a framework for scalable beacon interaction is at your
fingertips.

.. code:: swift

   override func viewDidLoad() {
       super.viewDidLoad()
       BlueCatsSDK.startPurringWithAppToken("app token", completion: nil//or check for required resources)
   }

   func beaconManager(beaconManager: BCBeaconManager!, didEnterSite site: [BCSite]!) {
       print("didEnterSite: \(site.name)")
       updateCurrentStoreWith(site) //perform some action
   }

Get started integrating the BlueCats SDK into your app: - `Installation
Guide for BlueCats SDK for
Android <https://developer.bluecats.com/guides/android-sdk-installation-48c4e341-ecd4-43f3-8f37-581e43fc53ab>`__
- `Installation Guide for BlueCats SDK for iOS (supports Swift or
Objective-C) <https://developer.bluecats.com/guides/ios-sdk-installation-c99b3b62-a271-4321-9c29-ce61699fce4f>`__

If this is your first time using beacons, check out our `Beacons
101 <%7B%7B'documentation/beacons/guides/beacons101'%20%7C%20relative_url%20%7D%7D>`__
guide.

In the BlueCats Platform, beacon context or meta-data is added to
beacons in two ways: through beacon specific information and through
hierarchical information assigned to a beacon through sites, categories
and custom values.

Beacon specific information is provided by the BlueCats platform and the
beacon itself. Examples of this include the mode of the beacon
i.e. Secure or Eddystone-URL, data from onboard sensors, iBeacon
Proximity UUID and battery status.

Beacon hierarchical information is provided by the BlueCats platform
through the placement and tagging of beacons within Teams, Sites, and
Categories. These elements can be used to trigger beacon interactions
and be used to sort and contextualize beacon visit data for analytics.
Beacon hierarchical information can be managed from the `BlueCats web
app <https://app.bluecats.com>`__ and REST API.
