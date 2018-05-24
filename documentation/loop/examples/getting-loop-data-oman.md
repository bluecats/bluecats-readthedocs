---
layout: documentation
title: Getting Data into Loop with OMAN 
---

*Current as of Version 0.3.3:*

This article will talk about how to create some objects in the Loop API and to start receiving data. There will be times that you may want to create or edit a large number of objects. 


## Getting Started - Getting Data into Loop

So let us say that you have 2 Beacons and 1 Edge that you want to put into Loop of an objectType. For our example we are going to make bcdEquipment. There are a few ways to go about this. Firstly, you want to make sure you have your Loop Login configuration file.

To create that file you will type the below command. Filling in your email and password, where it says `<email>` and `<password>`.

```
oman login --email=<email> --password=<password>

``` 

If all is well, you should receive back the response:

```
	 Loop API Login :
	 
 		created file auth.json
```


After you have created your Login config file, now we need to determine what objectTypes are available to us. So we are going to run the command: 

```
[
    "bcdEquipment",
    "bcdParcel",
    "bcdPerson",
    "bcdVehicle",
    "Device",
    "Place"
]
```

We will need to create an objectType `Device` for the Edge, 2 Beacons as as the objectType `bcdEquipment`, and a `Place` for that object to enter and exit. So we are going to pull down the objectType templates for Device, bcdEquipment, and Place and then fill them out. We will run these three commands with OMAN:

```
oman templateType create --forSelectionType=Place --fileName=Place.csv

oman templateType create --forSelectionType=Device --fileName=Device.csv

oman templateType create --forSelectionType=bcdEquipment --fileName=bcdEquipment.csv
```

Now we will open the file `bcdPerson.csv`. As you will see in the CSV, there are header for all of the keys. Below the keys, are what type of value type it is expecting and whether the value is required. 

This is shown in JSON format, because it is easier to present than CSV. 

```
[
    {
        "batterySoC": "int",
        "batterySoCObservedAt": "datetime",
        "groupID": "string editable  required ",
        "id": "string",
        "lat": "decimal",
        "long": "decimal",
        "mostRecentPlace": "string",
        "mrPlaceChangedAt": "datetime",
        "objectType": "bcdEquipment",
        "place": "string",
        "placeChangedAt": "datetime",
        "positionObservedAt": "datetime",
        "serialNumber": "string editable ",
        "temp": "int",
        "tempObservedAt": "datetime",
        "type": "string editable  required "
    }
]
```

Each objectType is different, but most of the time the only thing required to get the object to report to Loop is to add:

- objectType: What type of object it is and what values should be filled out. This is important to determine what properties/values or properties it has. 
- id: You do NOT need to fill in objectID. The objectID allows Loop to determine which object is which. The object ID can be automatically generated on creation. 
- groupID: Who has ownership and can see this object
 and eventID (need object ID to create).
- [eventID](https://bluecats.github.io/documentation/loop/events#internal-event-types): Object Links are what allow you to report events for a specific object to the Loop API. This can be bcId (bluecats identifier), Eddystone UID, iBeacon, bluetooth address, the Edge Mac, and more. 

Fill out all of the information, that you want. For our example, we will fill out groupID, Name, serialNumber, and leave objectType. In order to determine what groupID (what group has ownership) to use, we will need do a group query. 

``` 
oman query groups
```

This will return all of your groups and their groupIDs. 

```

 Loop groups :
[
    {
        "abbreviation": "bcdatx",
        "id": "3c68f2f14be643c28331a10cd91add35",
        "name": "BC ATX",
        "parentID": "a3cec63e2aa927c93567abd968677314"
    },
    {
        "abbreviation": "bcd",
        "id": "a3cec63e2aa927c93567abd968677314",
        "name": "BlueCats Labs",
        "parentID": "764166c0c49c4e113c62f0ca3cecd84e"
    }
]
```
Before we create our Device, we are going to create our bcdEquipment objectType. 

```
{
    "groupID": "a3cec63e2aa927c93567abd968677314",
    "name": "Loop Test Person",
    "objectType": "bcdPerson",
    "serialNumber": "50002CBA",
    "eventID": "eddystone"
}

```


## Getting the Event ID 

### EventID for BlueCats Beacons using Oman 

For eventID, you can put one of the following the fileheader: 
	- `ibeacon` for the Proximity UID 
	- `eddystone` for its Eddystone UID 
	- `bluetooth` for its Bluetooth Address
	- `bcId` for its SN.
	
For our case we are doing Eddystone UID, so we put eddystone at the eventID. 
We are going to pull down the Eddystone UID from BlueCats API. 

``` 
oman createEventID --fileName=bcdEquipment.csv
```

### EventID for BlueCats Beacons with BlueCats Web App

1. Go to the [BlueCats Web app](https://app.bluecats.com/)
2. Sign in 
3. Go to the Beacons Tab.

We are going to look for eddyston UID identifier. 
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

#### Where is Waldo? (Beacon style)

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
|iBeacon| iBeac#{iBeaconIDIdentifier} | `iBeac#5f2048a84e4d11e89c2dfa7ae01bbebc425005`|
|Eddystone UID | eddyUID#{eddyUIDIdentifier}|`eddyUID#AAFFAAFFAAFFAAFFAAFF00000001FC43`
| Bluetooth Address |btAddr#{eddyUIDIdentifier}|`btAddr#A0E6F854F61B` | 


#### Creating the Object 


```
oman create objects --objectLink=create --fileName=bcdPerson.json 
```

Now you should have your bcdPerson created in Loop. 
If it worked correctly, you should have received the response: 

```
 Loop post :
{
    "id": "f28769f32b94fdde64e438e4ca1d11dd",
    "objectType": "bcdPerson"
}

 Loop performed [post] on objects :

 created file bcdPerson2.json

 Loop post(ed) eventID(s) :
"success"
```

For the `Places.csv`, fill out all of the information, that you want. For our example, we will fill out city, fullName, groupID, name, postal code, street, and leave objectType. 


In order to determine what groupID (what group has ownership) to use, we will need do a group query. 

``` 
oman query groups
```

This will return all of your groups. 

```

 Loop groups :
[
    {
        "abbreviation": "bcdatx",
        "id": "3c68f2f14be643c28331a10cd91add35",
        "name": "BC ATX",
        "parentID": "a3cec63e2aa927c93567abd968677314"
    },
    {
        "abbreviation": "bcd",
        "id": "a3cec63e2aa927c93567abd968677314",
        "name": "BlueCats Labs",
        "parentID": "764166c0c49c4e113c62f0ca3cecd84e"
    }
]
```

Grab the groupID, in this case id: a3cec63e2aa927c93567abd968677314. Your groupID will be the groupID for your groups.

```
{
    "city": "Austin ",
    "fullName": "BlueCats Office Test",
    "groupID": "a3cec63e2aa927c93567abd968677314",
    "name": "BC Office",
    "objectType": "Place",
    "placeType": "Origin ",
    "postalCode": "78702 ",
    "street": "301 Chicon st, Suite A",
}
```

Now we are going to create the Place so we can get its objectID so that we can add it to our Device. 

```
oman create objects --fileName=Place.json 
```

The response will be:

```
 Loop post :
{
    "id": "bc7cc13c94aeb129819643e30e0cefe4",
    "objectType": "Place"
}

 Loop performed [post] on objects :

 created file Place.json
```


Now we have our objectID both in our file Place.json and in our terminal. We can fill out our Edge information with the deviceType, eventType, groupID, the objectID from our place, objectType, name, and eventID as edge in the `Device.json`.  

``` 
[
    {
        "deviceType": "WiFi Edge",
        "eventID": "edge",
        "groupID": "a3cec63e2aa927c93567abd968677314",
        "mac": "E4956E4CC077",
        "name": "Loop Edge Test",
        "objectType": "Device",
        "place": "bc7cc13c94aeb129819643e30e0cefe4"
    }
]
```

Adding the eventID for the Edge, you will run the OMAN command. 

```
oman createEventID --fileName=Device.json
```

After populating, the eventID now we can create our Device using this command.

```
oman create objects --objectLink=create --fileName=Device.json 
```

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
