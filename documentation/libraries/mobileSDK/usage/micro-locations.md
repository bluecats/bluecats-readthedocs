---
layout: documentation
title: Micro-Locations
---



A `BCMicroLocation` object represents the sites and beacons in proximity to the device. Observe `BCMicroLocationManager` notifications in iOS or implement `BCMicrolocationDelegate` / `BCMicroLocationManagerCallback` methods to handle beacon ranging events.

For more information on using BCMicroLocations, please see our guide [Using the BCMicroLocationManager](https://developer.bluecats.com/guides/using-the-bcmicrolocationmanager).

__Note: Micro-locations are deprecated in favor of BCTriggeredEvents and BCBeaconManager in SDK versions > 2.0.0.__

Before you will receive micro-location updates, you must send `startUpdatingMicroLocation()` to your BCMicroLocationManager. To stop receiving updates, call `stopUpdatingMicroLocation()`.

### Java
```java
BCMicroLocationManager.getInstance().startUpdatingMicroLocation( mMicroLocationManagerCallback );
BCMicroLocationManager.getInstance().stopUpdatingMicroLocation( mMicroLocationManagerCallback );

private BCMicroLocationManagerCallback mMicroLocationManagerCallback = new BCMicroLocationManagerCallback()
{
	...
}
```
### Objective-C
```objective-c
[[BCMicroLocationManager sharedManager] startUpdatingMicroLocation];
[[BCMicroLocationManager sharedManager] stopUpdatingMicroLocation];
```
### Swift
```swift
BCMicroLocationManager.sharedManager().startUpdatingMicroLocation()
BCMicroLocationManager.sharedManager().stopUpdatingMicroLocation()
```

## Get notified when nearby sites are updated
### Java
```java
@Override
public void onDidUpdateNearbySites( final List<BCSite> list )
{
	...
}
```
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateNearbySites:(NSArray *)sites {
  ...
}
```

### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateNearbySites sites: [AnyObject]!) {
  ...
}
```

## Get notified when the device enters a site
### Java
```java
@Override
public void onDidEnterSite( final BCSite bcSite )
{
	...
}
```
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didEnterSite:(BCSite *)site {
  ...
}
```
### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didEnterSite site: BCSite!) {
  ...
}
```

## Get notified when the device exits a site
### Java
```java
@Override
public void onDidExitSite( final BCSite bcSite )
{
	...
}
```
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didExitSite:(BCSite *)site {
  ...
}
```
### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didExitSite site: BCSite!) {
  ...
}
```

## Get notified when the device ranges beacons
### Java
```java
@Override
public void onDidRangeBeaconsForSiteID( final BCSite bcSite, final List<BCBeacon> list )
{
	...
}
```
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didRangeBeacons:(NSArray *)beacons inSite:(BCSite *)site {
  ...
}
```
### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didRangeBeacons beacons: [AnyObject]!, inSite site: BCSite!) {
  ...
}
```

## Get notified when the a new micro-location is available
### Java
```java
@Override
public void onDidUpdateMicroLocation( final List<BCMicroLocation> list )
{
	...
}
```
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateMicroLocations:(NSArray *)microLocations {
  ...
}
```
### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateMicroLocations microLocations: [AnyObject]!) {
  ...
}
```

## Get notified when micro-location services availability is updated (iOS only)
### Objective-C
```objective-c
- (void)microLocationManager:(BCMicroLocationManager *)microLocationManager didUpdateMicroLocationServicesAvailibility:(BOOL)available {
  ...
}
```
### Swift
```swift
func microLocationManager(microLocationManager: BCMicroLocationManager!, didUpdateMicroLocationServicesAvailibility available: Bool) {
  ...
}
```
