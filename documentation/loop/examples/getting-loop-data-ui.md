---
layout: documentation
title: Getting Data into Loop with the UI  
---

*Current as of Version 0.3.3:*

This article will talk about how to create some objects in the Loop API and to start receiving data. There will be times that you may want to create or edit a large number of objects. 

The way this tutorial is structured is for you to follow along so that you can get a greater understanding of how the Loop's UI works. If you have any problems or requests contact support@bluecats.com.

## Table of Contents 

- [Logging into Loop]()
- [Getting Data into Loop]()


## Logging into the Loop Dashboard

1. Firstly go ahead and login [here](http://bclabs.loop.bluecats.com/) 

	![Loop Login](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/loop-login.png)

2. You should be brought to the Loop homepage  
3. Once you are there, click on the top left icon (with three lines) to expand the menu. 

	![Menu Expander](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/menu-expander.png)

4. You should now be able to see the names of the icons. Click on Things. 

	![Menu Expanded](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/menu-expanded.png)

This should bring you to the Things Manager. 

![Things Manager](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/things-manager.png)

The next few sections will be explaining how to enter objects into loop. 


## Getting Started - Getting Data into Loop

So let us say that you have 1 Beacon (BlueCats or a 3rd Party) and 1 Edge that you want to put into Loop of an objectType. For our example we are going to make one Beacon that is of the type Equipment, and the Edge will be a Device to track the data. 

## Creating Objects

1. First we are going to click on the 'Create' button.

	![Create Things](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/create-things.png)
 
2. This will bring us to a screen that will allow us to choose what group owns the object and also what type of object it is. 

	![Create Things Menu](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/create-thing-menu.png)
	
3. For our purposes, we are going to select BlueCats Labs. It is likely you will have different groups here. 

### Group Hierarchy  
	
The groups have a hierarchy. For example, if something has a hierarchy like this:
		 
	
	```
			BlueCats
				BlueCats Labs 
				BlueCats Austin
	```
	
BlueCats will have ownership of everything underneath it, which is everything that BlueCats Labs and BlueCats Austin have. However, BlueCats Labs cannot see everything BlueCats owns or what BlueCats Austin owns. 
		
You will probably have a good idea of your group's hierachy and structure. If not, think about it before, but don't worry. You can always change it. 

Moving on, after selecting your group, you are going to select your thing's objectType. We are using the objectType Equipment. You most likely have different objectTypes. 

### Determining Object Types
	
You will probably have different objectTypes available to you. You will have to discuss this with your organization with how you want to set this up. Try to match the objectType best with what you are tracking. Like Equipment for a thing or Person for a Person. Loop does not care what you pick. It is entirely up to you. Don't worry if you can't decide! You can always change it. 
	
![Equipment Select](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/equipment-select.png)

5. For the next part, you probably will have different fields available to you. Try to fill those in the best you can. For Type, we are going to put Car for this example and the Beacon's serial Number `50000B8A`.
6. Then click Save. 

	![Equipment Save](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/equipment-save.png)

So some of you might be saying, woah there!! I have a 3rd party Beacon. What am I supposed to do with that? Well, for now if you do not have Bluetooth MAC or anything for that Beacon, put a placeholder like equipment1. We will change that to the 3rd party Beacon's Bluetooth MAC address later. 


## Link Identifiers

[Link Identifiers](https://bluecats.github.io/documentation/loop/events#internal-event-types) are what allow you to report events for a specific object to the Loop API. This can be bcId (bluecats identifier), Eddystone UID, iBeacon, bluetooth address, the Edge Mac, and more. 

### EventID for BlueCats Beacons with BlueCats Web App

1. Go to the [BlueCats Web app](https://app.bluecats.com/)
2. Sign in 
3. Make sure you have selected your Team. If it is not, click the menu and click the right team for, which your Beacon belongs. 
 
	![Beacon Menu Select](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/bc-team-select.png)
 
3. Go to the Beacons Tab.

	![Beacon Menu Select](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/beacon-menu-select.png)

4. Go to the search bar and type in your Beacon serial number

	![Beacon Search](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/beacon-search.png)

5. You should find your Beacon and it should look like this 

	![Beacon Found](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/beacon-found.png)

We are going to look for Eddystone UID identifier. If your Beacon is in the right mode, then it should show the identifier you are looking for. If not, put your Beacon in the right mode and update it with BC Reveal. 

These are the main types of eventIDs available [identifier](https://bluecats.github.io/documentation/loop/events#event-identifiers). It depends what mode your beacons are in:


#### iBeacon 

For sending iBeacon ads to Loop, look for its proximity UID. Remove all of the hyphens and colons. So from this 5f2048a8-4e4d-11e8-9c2d-fa7ae01bbebc:4:25005 to this 5f2048a84e4d11e89c2dfa7ae01bbebc425005.

You would put this for eventID:  `iBeac#5f2048a84e4d11e89c2dfa7ae01bbebc425005`

#### Eddystone UID 

For sending Eddystone ads to Loop, look for its namespace and instance. Remove all of the hyphens and colons. So from this AAFFAAFFAAFFAAFFAAFF00000001FC43. 

You would put this for your eventID: `eddyUID#AAFFAAFFAAFFAAFFAAFF00000001FC43`

#### Bluetooth Address 

For sending secure ads to Loop, look for its Bluetooth Address with it will look like: `A0E6F854F61B`. To use this identifier type, your Beacon should be in Secure mode. Note that the public mac and the BT Address is different when Privacy Duration is not set to 0. 

You would put this for your eventID: `btAddr#A0E6F854F61B`

#### BlueCats Identifier

- For sending the BlueCats identifier, look at the BlueCats Beacon's Serial number. The serial number should be located on the side. To use this identifier type, your Beacon should be in Secure mode.

You would put this for your eventID: `serNum#30000DF5`

### EventID for 3rd-Party Beacons 

For 3rd-Party Beacons, it is a little bit more involved. Each 3rd Party Beacon will be a little different, but these instructions will go over what you need to do to find out it's information. 

We will need to set up our Edge Relay to determine which Beacons are associated with which identifiers

1. Log into Edge Portal [192.168.8.1](http://192.168.8.1)
2. Go to BlueCats > Advanced 
3. Click 'Scan Unknown Bluetooth Advertisements'. This will allow you to 

	![Scan Unknown BLE Ads](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/scan-uknown-BT-devices.png)

4. Click `Save & Apply`
5. Now, go to BlueCats > Live View

#### Where's Waldo? (Finding Info on 3rd Party Beacons)

Once you open Live View, if you have a ton of BLE devices near you. You will have to use the strongest RSSI values to determine which is closest. In most cases, when you do not know the identifiers. For 3rd-Party beacons, there are three common use cases that the Beacons will be advertising: mac (Bluetooth MAC address), iBeac (iBeacon), UID (Eddystone UID), Unk (unknown). 

Remove other Beacons as far as reasonably possible. Now place the Beacon very close to the Edge Relay. In our example, we have a Milwaukee Tick. 


<p align="center"><img width="350px" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/3rdParty-BLE-close-pic.jpg"/></p>

Take a look at the MAC address, RSSI, and the adTypes. So because we have multiple Beacons around us, you will see that one RSSI (Received Strength Signal Indicator) value is a very high number -45. Something to note is the more negative the less negative number the higher the RSSI value is. For example, -23 is higher than -123. 

From this we found this very high RSSI value and suspect that this is the Beacon in question. 

![3rd Party Close Beacon RSSI](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/3rdParty-BLE-close-RSSI.png)


We are next going to move this Beacon further away similarly to the picture below. 

![3rd Party Far Beacon](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/3rdParty-BLE-far-pic.jpg)

A lot of Third Party Beacons have slower ad rates, so it might take longer for them to update. Once it does, Then you can confirm that this Beacon was the one in question. As our picture below shows, the RSSI value increases. 

![3rd Party Far Beacon RSSI](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/3rdParty-BLE-far-RSSI.png)

Congratulations! You found Waldo!

#### Plugging Waldo into Loop 

Once you determine which Beacon yours is, you will need to determine, which Ad-type is available and the format them for your eventID. Each 3rd-Party Beacon will be different, we cannot really recommend which advertisement will be best. We have found very slow rates for some of their advertisements (i.e. slow iBeacon rates), but faster in other cases. This is for manually adding in eventIDs for Loop.

|Ad-Type| Formatted eventID| Example
|-------|------------------|------------|
|iBeacon| {iBeaconIDIdentifier} | `5f2048a84e4d11e89c2dfa7ae01bbebc425005`|
|Eddystone UID | {eddyUIDIdentifier}|`AAFFAAFFAAFFAAFFAAFF00000001FC43`
| Bluetooth Address |{eddyUIDIdentifier}|`A0E6F854F61B` | 

A recommendation we have, is once you find the Bluetooth MAC address for a 3rd Party Beacon. Plug that MAC address as the Serial Number field if it is required. Also, if you have a lable maker. Labeling the 3rd party Beacons with the MAC address is usually pretty helpful. 

## Plugging the Link Identifier into Loop

Going back to our Loop tab: 

1. Now we are going to click on the Beacon radio button (pictured below) so that we can create a link identifier for it. 
	
	![Beacon Link Select](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/beacon-link-select.png)

2. Next we are going to select the identifer type. For our example, we are going to select Eddystone. 
	
	![Beacon Link Identifier](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/beacon-link-types.png)

3. We are going to paste the identifier in the field

	![Beacon Link Identifier](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/link-uid.png)

4. Now, we are going to click Link at the bottom of the page. 
5. Once we click link, we should see a link identifier show up like this.

	![Beacon Link Identifier](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/link-identifier.png)


You can technically have multiple types of link identifiers if you want. But for most cases you should only need one. 

Now that we have our object set up, we are going to finally set up our Edge Relay by creating a Device in Loop and then have the Edge forward the Data to Loop. 

<!---

Now that we have our object set up. Let's create our Place so that our object can enter a place with the Edge Relay. 

## Creating a Place

We need to create a place so that when we create our Device our loop object can enter... a place! 

1. First we are going to click on the tab Places
	
	![Places Select](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/places-select.png)

2. Then we are going to click create button 

	![Places Create](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/places-create.png)
	
3. We need to determine who will own this Place.
4. Once you select which group will own the Place, you will need to fill out your place fields. If you do not use the abbreviations for state/province or country codes. It may throw an error at you. 

	![Places Field](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/places-field2.png)

5. Once you fill out the Place fields, click Save. 

	![Save](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/save+button.png)

6. You should now see your newly created Place! 
7. We will need the Places ID, so click on Edit like the picture below 
	
	![Places Edit](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/place-edit.png)

8. Highlight and copy the ID. We will need that again shortly. 
	
	![Places ID](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/place-id.png)

Now that we set up our place, we are going to finally set up our Edge Relay by creating a Device in Loop and then have the Edge forward the Data to Loop. 

!-->

## Setting Up the Edge Relay 

1. First we are going to click on the 'Create' button.

	![Create Things](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/create-things.png)
 
2. This will bring us to a screen that will allow us to choose what group owns the object and also what type of object it is. 

	![Create Things Menu](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/create-thing-menu.png)
	
3. For our purposes, we are going to select BlueCats Labs. It is likely you will have different groups here. 
4. Fill out all of the Edge fields that are pertinent to you. 

	![Create Things Menu](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/edge-fields.png)

5. Next, click save. 

	![Save](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/save+button.png)
	
6. After, setup you can setup the Edge link identifier. The Edge just needs the Edge MAC located on the bottom. 
	

	![Edge Mac](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/edge-mac.JPG)

7. Once you locate the Edge MAC, click on the Edge Relay radio button. Fill in the Edge MAC, then remove colons. 

	![Edge Link Mac Setup](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/edge-link-setup.png)
	
8. Then click Link, and your edge-link will look like this. 

	![Edge Link Identifier](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/edge-link-identifier.png)
	

## Sending Data to Loop with the Edge

Lastly, we will need to set up our Edge Relay to send its data to Loop. 

1. Log into Edge Portal [192.168.8.1](http://192.168.8.1)

![Log in to Edge Web Configuration](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png "Log in")

- Check to make sure that your Edge's firmware version is up to date. 
	8. Here is a link to show you how to check your [firmware version](https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/getting-started-connect#edge-web-interface--checking-your-firmware-version). 
	9. It should have a firmware version range from (0.2.X-0.4.X). 
	10. If your Edge's firmware is out of date, you should update it following one of the two procedures [here](https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/edge-update-firmware). 
- You should also make sure that you are connected to the internet.
	12. Here is how to a tutorial on [Connecting your Edge to WiFi](https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/getting-started-connect#connecting-to-internet-via-wifi)

6. Now that we checked ourFor the Edge MAC, set the zone to the corresponding Edge Mac that you are forwarding it to

![Shared Scan Heartbeats](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/loop-shared-scan.png)

7. Go to BlueCats > Forwarding
13. Add a new endpoint 
14. Name it “shared hb” or “shared Heartbeat”
15. Check Enabled
16. Event type should be Heartbeat Events
17. Protocol should be HTTP
18. Ad type to identify beacon in JSON: 
	19. Can be `Secure, Eddy UID, iBeacon or bcId`
	20. In this case, pick Eddy UID
19. For Host IP address put: shared.api.loop.bluecats.com/events
20. For the port, pick 443
21. For Interval length, type: 900 seconds. 
23. Click “Save and Apply”
13. Add a new endpoint 
14. Name it “Shared Scan” or "Shared Scan BLE"

![Shared Scan Heartbeats](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/loop-shared-hb.png)

15. Check Enabled
16. Event type should be BLE Scan Events
17. Protocol should be HTTP
18. Ad type to identify beacon in JSON: 
	19. Can be `Secure, Eddy UID, iBeacon or bcId`
	20. In this case, pick Eddy UID
19. For Host IP address put: shared.api.loop.bluecats.com/events
20. For the port, pick 443
21. Number Per Interval: Put at least 1000 
21. For Interval length, type: 300 seconds. 
23. Click “Save and Apply”

You should now be able to send information to Loop!

After the designated interval time, check Loop under Objects. 
Click View on your object. 

![Create Things](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/object-check.png)


After you bring up your object, you should now be able to see some events in Loop! 


![Create Things](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/object-events.png)

