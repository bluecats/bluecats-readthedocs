---
layout: documentation
title: SDK Options
---

The BlueCatsSDKs have global options to customise their behaviour to best suit your deployment. **Be mindful when selecting your options.** In most cases, the default SDK options are preferred.

For example, to prevent the Bluetooth power warning message in iOS, not pre-cache beacons based on device location, and reduce the local cache refresh interval to ten minutes you would set the following options.

##### Java
```java
final Map<String, String> options = new HashMap<>();
options.put( BlueCatsSDK.BC_OPTION_DISCOVER_BEACONS_NEARBY, "false" );
options.put( BlueCatsSDK.BC_OPTION_CACHE_REFRESH_TIME_INTERVAL_IN_MILLISECONDS, "600000" ); //10 minutes

BlueCatsSDK.setOptions( options );
```
##### Objective-C
```objective-c
[BlueCatsSDK setOptions:@{
        BCOptionShowBluetoothPowerWarningMessage:@"NO",
                   BCOptionDiscoverBeaconsNearby:@"NO",
       BCOptionCacheRefreshTimeIntervalInSeconds:@"600"}]; //10minutes
```
##### Swift
```swift
BlueCatsSDK.setOptions([
    BCOptionShowBluetoothPowerWarningMessage: "NO",
    BCOptionDiscoverBeaconsNearby: "NO",
    BCOptionCacheRefreshTimeIntervalInSeconds: "600"]) //10 minutes
```

### Both SDKs

**BCOptionApiBaseURL** / **BC_OPTION_API_BASE_URL**  
Default: Unused - Uses the standard BlueCats API URL.  
Type: String

If using a custom URL route, input this here.

**BCOptionCacheAllBeaconsForApp** / **BC_OPTION_CACHE_ALL_BEACONS_FOR_APP**  
Default: `NO` / `false`  
Type: Boolean

When enabled, the SDK will cache all the beacons and sites belonging to the app's team. This is useful when you have a small number of beacons (e.g. less than a few hundred) that you want always available to the app, even when the device is offline or loses network connectivity.

**BCOptionCacheRefreshTimeIntervalInSeconds** / **BC_OPTION_CACHE_REFRESH_TIME_INTERVAL_IN_MILLISECONDS**  
Default: `600` / `600,000` - 10 minutes  
Minimum value: `30` / `30,000` - 30 seconds  
Type: Integer / Long

Sets the interval in which beacons and sites are refreshed while the app is running. Updated beacons and sites are also requested when the app is launched to the foreground or when a device enters a site.

See [Optimising your Caching Strategy](https://developer.bluecats.com/guides/optimising-your-caching-strategy-896007fc-4811-4584-8659-36e2f7143674) for more information on choosing a caching option.

**BCOptionCacheSitesNearbyByLocation** / **BC_OPTION_CACHE_SITES_NEARBY_BY_LOCATION**  
Default: `NO` / `false`  
Type: Boolean

This option controls caching of beacons in nearby sites based on significant location change. With this feature disabled, metadata for beacons in a site is cached when the first detected beacon is detected by the phone. Disabling this feature reduces the frequency that the app will be woken up for significant location changes and subsequent network requests when not in range of beacons.

**BCOptionCrowdSourceBeaconUpdates** / **BC_OPTION_CROWD_SOURCE_BEACON_UPDATES**  
Default: `YES` / `true`  
Type: Boolean

Allows user devices to update beacon settings autonomously when in range of a beacon. This is useful if beacons need to be updated but is impractical for a single person to do manually.

**BCOptionDiscoverBeaconsNearby** / **BC_OPTION_DISCOVER_BEACONS_NEARBY**  
Default: `YES` / `true`  
Type: Boolean

Beacons are loaded and cached on demand in batches relevant to a device's distance from a site. This is useful when there are a large number of beacons spread over a large geographical region where only a subset of beacons are relevant to a particular users' location.

**BCOptionUseEnergySaverScanStrategy** / **BC_OPTION_ENERGY_SAVER_SCAN_STRATEGY**  
Default: `YES` / `true`
Type: Boolean

A setting to help save battery life. When disabled the SDK will more aggressively scan for beacons.

**BCOptionMonitorAllAvailableRegionsOnStartup** / **BC_OPTION_MONITOR_ALL_AVAILABLE_REGIONS_ON_STARTUP**  
Default: `YES` / `true`  
Type: Boolean

This option directs the SDK to begin ranging all regions available to the app on startup.

**BCOptionMonitorBlueCatsRegionOnStartup** / **BC_OPTION_MONITOR_BLUE_CATS_REGION_ON_STARTUP**  
Default: `YES` / `true`  
Type: Boolean

This option enables ranging of beacons in the default BlueCats region (Proximity UUID: 61687109-905F-4436-91F8-E602F514C96D) on startup.

**BCOptionTrackBeaconVisits** / **BC_OPTION_BEACON_VISIT_TRACKING_ENABLED**  
Default: `YES` / `true`  
Type: Boolean  

Enables the tracking of beacon visits. If you do not want beacon visits collected set this to `NO` / `false`.

**BCOptionUseLocalStorage** / **BC_OPTION_USE_LOCAL_STORAGE**  
Default: `YES` / `true`  
Type: Boolean

Enables ranging of previously cached beacons from the local SQLite database when network connectivity is not available.

**BCOptionUseApi** / **BC_OPTION_USE_API**  
Default: `YES` / `true`  
Type: Boolean

Allows for the use of SDK interaction without interaction with the BlueCats API.

### Android Specific
**BC_OPTION_DEEP_SLEEP_WAKEUP_TIME_INTERVAL_IN_MINUTES**  
Minimum: `1` minute  
Default: `5` minutes  
Type: Integer  

Keeps the SDK running in the background by waking it up every `x` minutes to scan.

**BC_OPTION_USE_RSSI_SMOOTHING**  
Default: `true`  
Type: Boolean  

This uses smoothing of the RSSI value to produce a more stable RSSI signal strength, removing much of the jitter typically returned by the Bluetooth scanner. As a result there is approximately a 3 second delay in updating a beacons distance. You can set this to false and use `BCEventFilter.filterApplySmoothedRSSIOverTimeInterval( ms )` to apply a custom value to the smoothing if desired.

**BC_OPTION_USE_STAGE_API**  
Default: `false`  
Type: Boolean  

Use the BlueCats staging API rather than the regular API.

**BC_OPTION_REMOTE_CONFIGURATION_EXPIRATION_TIME_INTERVAL_IN_MILLISECONDS**  
Minimum: `1`
Default: `259200000` (3 days)
Type: Long

Use this option to set the expiration time for the remote configuration.

**BC_OPTION_REMOTE_CONFIGURATION_REFRESH_TIME_INTERVAL_IN_MILLISECONDS**  
Minimum: `1`
Default: `600000` (10 minutes)
Type: Long

Use this option to set the refresh time for the remote configuration.

### iOS Specific
**BCOptionAutoTrackStandardEvents**  
Default: `YES`  
Type: Boolean

This option directs the SDK to log the following events when they occur:
* startPurring  
* stopPurring  
* applicationWillResignActive  
* applicationDidEnterBackground  
* applicationWillEnterForeground  

**BCOptionBackgroundSessionTimeIntervalInSeconds**  
Default: None - continuous ranging  
Type: Integer

Sets the maximum length of a continuous background session. This is used to limit the amount of time the SDK is ranging beacons while the app is in the background to preserve battery life.

**BCOptionMaximumDailyBackgroundUsageInMinutes**  
Default: `1,440` (one day)
Type: Integer

If set, the SDK will limit the daily background usage to this value. Use this setting to help conserve battery usage for users who may be in range of your beacons for significant periods of time.

**BCOptionScanInBackground**  
Default: `YES`  
Type: Boolean

This option enables the SDK to scan for beacons in the background. Leaving this option as `YES` is especially important if you want to trigger actions when an app is closed or in the background.

**BCOptionShowBluetoothPowerWarningMessage**  
Default: `YES`  
Type: Boolean

iOS only. This will prompt the user to turn on their Bluetooth.
