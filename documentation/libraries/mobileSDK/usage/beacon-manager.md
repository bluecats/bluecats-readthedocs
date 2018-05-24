---
layout: documentation
title: Beacon Manager
---

The beacon manager is the central point for configuring the delivery of beacon ranging events to your app. Use this class if you wish to create reactions to individual beacons or beacons in particular modes.

If you haven't installed our SDK, head over to the installation pages for [Android](https://github.com/bluecats/bluecats-android-sdk) or [iOS](https://github.com/bluecats/bluecats-ios-sdk).

**Note: The beacon manager is only available in SDK versions > 2.0.0.**

Once purring, the BlueCats SDK begins scanning for all beacons the app has been granted access to. If you wish to scan for beacons outside of the BlueCats platform, create new BCBeaconRegions and use the `startMonitoringBeaconRegion` method.

#### Beacon Manager Set-up
##### Java
```java
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
```
##### Objective-C
```objective-c
#import "BlueCatsSDK/BlueCatsSDK.h" //import the main header or individual headers

@interface ViewController () <BCBeaconManagerDelegate>
@property (strong, nonatomic) BCBeaconManager *beaconManager;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    self.beaconManager = [[BCBeaconManager alloc] initWithDelegate:self queue:nil];
}
```
##### Swift
```swift
import BlueCatsSDK //if using the framework, otherwise this is unneeded

class ViewController: UIViewController, BCBeaconManagerDelegate {

    var beaconManager = BCBeaconManager()

    override func viewDidLoad() {
        super.viewDidLoad()

        beaconManager = BCBeaconManager.init(delegate: self, queue: nil)
    }
```

#### Get notified when the device comes into the range of beacons
##### Java
```java
@Override
public void didEnterBeacons( final List<BCBeacon> beacons )
{
	Log.d( TAG, "didEnterBeacons: " + beacons.size() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didEnterBeacons:(NSArray<BCBeacon *> *)beacons {
  NSLog(@"didEnterBeacons: %lu", (unsigned long)[beacons count]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didEnterBeacons beacons: [BCBeacon]!) {
	print("didEnterBeacons: \(beacons.count)")
}
```

#### Get notified when new beacon ranging information is available
##### Java
```java
@Override
public void didRangeBeacons( final List<BCBeacon> beacons )
{
	Log.d( TAG, "didRangeBeacons: " + beacons.size() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didRangeBeacons:(NSArray<BCBeacon *> *)beacons {
  NSLog(@"didRangeBeacons: %lu", (unsigned long)[beacons count]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didRangeBeacons beacons: [BCBeacon]!) {
	print("didRangeBeacons: \(beacons.count)")
}
```

#### Get notified when the device exits the range of beacons
##### Java
```java
@Override
public void didExitBeacons( final List<BCBeacon> beacons )
{
	Log.d( TAG, "didExitBeacons: " + beacons.size() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didExitBeacons:(NSArray<BCBeacon *> *)beacons {
  NSLog(@"didExitBeacons: %lu", (unsigned long)[beacons count]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didExitBeacons beacons: [BCBeacon]!) {
	print("didExitBeacons: \(beacons.count)")
}
```

#### Get notified when the device enters a site
##### Java
```java
@Override
public void didEnterSite( final BCSite site )
{
	Log.d( TAG, "didEnterSite: " + site.getName() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didEnterSite:(BCSite *)site {
  NSLog(@"didExitBeacons: %lu", (unsigned long)[beacons count]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didEnterSite site: [BCSite]!) {
	print("didEnterSite: \(beacons.count)")
}
```

#### Get notified when the device exits a site
##### Java
```java
@Override
public void didExitSite( final BCSite site )
{
	Log.d( TAG, "didExitSite: " + site.getName() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didExitSite:(BCSite *)site {
  NSLog(@"didExitSite: %lu", (unsigned long)[beacons count]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didExitSite site: [BCSite]!) {
	print("didExitSite: \(beacons.count)")
}
```

#### Get notified when the SDK ranges an Eddystone beacon
##### Java
```java
@Override
public void didRangeEddystoneBeacons( final List<BCBeacon> eddystoneBeacons )
{
	Log.d( TAG, "didRangeEddystoneBeacons: " + eddystoneBeacons.size() );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didRangeEddystoneBeacons:(NSArray<BCBeacon *> *)beacons {
  NSLog(@"didExitSite: %lu", (unsigned long)[beacons count]);
}
```
##### swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didRangeEddystoneBeacons beacons: [BCBeacon]!) {
  print("didRangeEddystoneBeacons: \(beacons.count)")
}
```

#### Get notified when the SDK discovers an Eddystone URL
##### Java
```java
@Override
public void didDiscoverEddystoneURL( final URL eddystoneUrl )
{
	Log.d( TAG, "didDiscoverEddystoneURL: " + eddystoneUrl );
}
```
##### Objective-C
```objective-c
- (void)beaconManager:(BCBeaconManager *)beaconManager didDiscoverEddystoneURL:(NSURL *)eddystoneURL {
  NSLog(@"didDiscoverEddystoneURL: %@", [eddystoneURL absoluteString]);
}
```
##### Swift
```swift
func beaconManager(beaconManager: BCBeaconManager!, didDiscoverEddystoneURL eddystoneURL: NSURL!) {
  print("didDiscoverEddystoneURL: \(eddystoneURL.absoluteString)")
}
```
