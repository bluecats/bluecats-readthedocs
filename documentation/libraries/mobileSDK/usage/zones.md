---
layout: documentation
title: Zones
--- 

The `BCZone` class defines an object that represents a dynamic micro-location. A `BCZone` can be composed of a site, a category, multiple categories and specific beacons. Zones can also be configured to trigger dwell time events.

### Initializing the Zone Monitor
Custom values placed on beacons, categories and sites define which sites are included within a zone. You must pass the key for these custom values to the zone monitor upon initialization.

##### Java
```java
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
```
##### Objective-C
```objective-c
NSArray *zoneIdentifierKeys = @[@"ZONE_ID_KEY";
_zoneMonitor = [[BCZoneMonitor alloc] initWithDelegate:self queue:nil zoneIdentifierKeys:keys];
[_zoneMonitor startMonitoringZones];
```
##### Swift
```swift
let zoneIdentifierKeys = ["ZONE_ID_KEY"]
zoneMonitor = BCZoneMonitor.init(delegate: self, queue: nil, zoneIdentifierKeys: zoneIdentifierKeys)
zoneMonitor.startMonitoringZones()
```

#### Get notified when the device enters a site
##### Java
```java
@Override
public void didEnterSite( final BCSite site )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didEnterSite:(BCSite *)site {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didEnterSite site: BCSite!) {
	...
}
```

#### Get notified when the device enters a zone
##### Java
```java
@Override
public void didEnterZone( final BCZone zone )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didEnterZone:(BCZone *)zone {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didEnterZone zone: BCZone!) {
	...
}
```

#### Get notified when the device re-enters a zone
##### Java
```java
@Override
public void didReenterZone( final BCZone zone )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didReEnterZone:(BCZone *)zone {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didReEnterZone zone: BCZone!) {
	...
}
```

#### Get notified when the device ranges beacons in a zone
##### Java
```java
@Override
public void didRangeBeacons( final List<BCBeacon> beacons, final BCZone inZone )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didRangeBeacons:(NSArray *)beacons inZone:(BCZone *)zone {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didRangeBeacons beacons: [AnyObject]!, inZone zone: BCZone!) {
	...
}
```

#### Get notified when your dwell time interval is met
Dwell time events occur when the device dwells in the specified zone matching the dwell time interval.
##### Java
```java
@Override
public void didDwellInZone( final BCZone zone, final Integer dwellTimeInterval )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didDwellInZone:(BCZone *)zone forTimeInterval:(NSTimeInterval)dwellTimeInterval {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didDwellInZone zone: BCZone!, forTimeInterval dwellTimeInterval: NSTimeInterval) {
	...
}
```

#### Get notified when the device exits a zone
##### Java
```java
@Override
public void didExitZone( final BCZone zone )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor didExitZone:(BCZone *)zone {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, didExitZone zone: BCZone!) {
	...
}
```

#### Get notified when zone monitoring is suspended
##### Java
```java
@Override
public void willSuspendMonitoringInSite( final BCSite site, final Date untilDate )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor willSuspendMonitoringInSite:(BCSite *)site untilDate:(NSDate *)date {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, willSuspendMonitoringInSite site: BCSite!, untilDate date: NSDate!) {
	...
}
```

#### Get notified when zone monitoring will resume
##### Java
```java
@Override
public void willResumeMonitoringInSite( final BCSite site )
{
	...
}
```
##### Objective-C
```objective-c
- (void)zoneMonitor:(BCZoneMonitor *)monitor willResumeMonitoringInSite:(BCSite *)site {
	...
}
```
##### Swift
```swift
func zoneMonitor(monitor: BCZoneMonitor!, willResumeMonitoringInSite site: BCSite!) {
	...
}
```
