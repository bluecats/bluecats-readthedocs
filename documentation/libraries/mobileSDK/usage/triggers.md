---
layout: documentation
title: Triggers
---

Triggers and events allow you to trigger actions in specific micro-locations. Combine BCEventFilters into a BCTrigger to trigger events easily after your filters are satisfied.

BCEventFilters are applied in the order they're inserted. If any of them fail, the trigger won't fire.

Filters include:
 * proximity, accuracy and RSSI filtering
 * category and site filtering
 * closest beacon changed
 * accuracy and RSSI smoothing
 * dwell based triggers

The examples below cover using triggers at a high level, for a more through explanation see our guide [Using Triggers and Events](https://developer.bluecats.com/guides/using-triggers-and-events).

#### Trigger Creation and Usage
##### Java
```java
private void createBasicTrigger()
{
	//generate a trigger with a random identifier
	final BCTrigger trigger = new BCTrigger();

	//add your filters
	//filter by sites
	trigger.addFilter( BCEventFilter.filterBySitesNamed( Arrays.asList(
		"BlueCats Cafe"
	) ) );

	//filter to within 10cm
	trigger.addFilter( BCEventFilter.filterByAccuracyRangeFrom( 0.0, 0.1 ) );

	//repeat indefinitely, or however many times you want
	trigger.setRepeatCount( Integer.MAX_VALUE );

	//add your trigger to the event manager
	BCEventManager.getInstance().monitorEventWithTrigger( trigger, mEventManagerCallback );
}

private final BCEventManagerCallback mEventManagerCallback = new BCEventManagerCallback()
{
	@Override
	public void onTriggeredEvent( final BCTriggeredEvent bcTriggeredEvent )
	{
		//do something with the filtered micro-location here when the trigger is fired
	}
}
```
##### Objective-C
```objective-c
- (void)createBasicTrigger
{
    //generates a trigger with a random identifier
    BCTrigger *trigger = [[BCTrigger alloc] init];

    //add your filters
    //filter by site
    [trigger addFilter:[BCEventFilter filterBySitesNamed:@[@"Cat Cafe"]]];
    //filter to within 10cm
    [trigger addFilter:[BCEventFilter filterByAccuracyRangeFrom:0.0 to:0.1]];

    //repeat indefinitely, or however many times you want
    trigger.repeatCount = NSIntegerMax;

    //add your trigger to the event manager
    [[BCEventManager sharedManager] monitorEventWithTrigger:trigger];
}

-(void)eventManager:(BCEventManager *)eventManager triggeredEvent:(BCTriggeredEvent *)triggeredEvent
{
    //do something with the filtered micro-location here when the trigger is fired
}
```
##### Swift
```swift
func createBasicTrigger(){

		//generates a trigger with a random identifier
		let trigger: BCTrigger! = BCTrigger()

		//add your filters
		//filter by site
		trigger.addFilter(BCEventFilter.filterBySitesNamed(["Cat Cafe"])))
		//filter to within 10cm
		trigger.addFilter(BCEventFilter.BCEventFilter.filterByAccuracyRangeFrom(0.0, to: 0.1))

		//repeat indefinitely, or however many times you want
		trigger.repeatCount = NSIntegerMax

		//add your trigger to the event manager
		BCEventManager.sharedManager().monitorEventWithTrigger(trigger)
}

func eventManager(eventManager: BCEventManager!, triggeredEvent: BCTriggeredEvent!) {
		//do something with the filtered micro-location here when the trigger is fired
}
```
