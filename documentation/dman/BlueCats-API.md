---
layout: documentation
title: DMAN API
category: Device Management
order: 1
---

This describes the resources for the DMAN API. If you have any problems or requests contact support@bluecats.com.

The BlueCats DMAN API allows you to: 

- GET Resources
- PATCH and PUT Resources

## Table of Contents:

- [Connecting to the API](https://bluecats.github.io/documentation/dman/BlueCats-API#connecting-to-the-api)
- [404 Errors](https://bluecats.github.io/documentation/dman/BlueCats-API#404-errors)
- [Obtaining Credentials](https://bluecats.github.io/documentation/dman/BlueCats-API#obtaining-credentials)
	- [OAuth 2.0 Credentials](https://bluecats.github.io/documentation/dman/BlueCats-API#oauth-20-credentials)
	- [Credentials from the BlueCats Web App](https://bluecats.github.io/documentation/dman/BlueCats-API#credentials-from-the-bluecats-web-app) 
- [API Endpoints](https://bluecats.github.io/documentation/dman/BlueCats-API#api-endpoints)
	- [GET Resources](https://bluecats.github.io/documentation/dman/BlueCats-API#get-resources)
	- [PUT Resources](https://bluecats.github.io/documentation/dman/BlueCats-API#put-resources)
	- [PATCH Resources](https://bluecats.github.io/documentation/dman/BlueCats-API#patch-resources)
	- [Editable Beacon Settings](https://bluecats.github.io/documentation/dman/BlueCats-API#editable-beacon-settings)

## Connecting to the API 

| Environment   | Connectivity | PORT |
|---------------|--------------|------|
| *BlueCats API*| URL: https://api.bluecats.com/ | 443 (default https port)|

## 404 Errors 

If the API returns a 404 error - `cat got your tongue`, this means that you hit a bad endpoint. So make sure the endpoint you are hitting is correct.  

## Obtaining Credentials

### OAuth 2.0 Credentials  

Authentication with the BlueCats API follows the key/secret usage outlined in the [OAuth 2.0 Client Credentials Flow](https://tools.ietf.org/html/rfc6749#section-4.4). Don't leak your OAuth application's client secret to application users - this is intended for use in server to server scenarios.

Receive your bearer token by posting your your client ID and secret to `https://api.bluecats.com/token` using the `client_credentials` grant type.

You must send as the header `ContentType=application/x-www-form-urlencoded`.
The body should be as form data in x-www-form-urlencoded: 

```Python
grant_type=client_credentials

client_id={client_id}

client_secret={client_secret}
```

### Credentials from the BlueCats Web App 


To obtain credentials:

1. Go to the [BlueCats Web App](https://app.bluecats.com)
2. Go to the app tab then "Create a new app" 
3. Give a name to your app. 
4. Select the platform: API Client
5. Once you create your app, click "Show App Token/Secret"
6. Click "Reset Client Secret"
7. Write down the Client ID and Client Secret

You will need to Base64 encode the Client ID and Client Secret in the form: {clientID}:{clientSecret}

You can either Base64 encode using a website like [Base64 Encode](https://www.base64encode.org) or if you want to use this short Python example below to encode your credentials. 

```python
import base64

client_id = ""
client_secret = ""
raw_auth = client_id + ":" + client_secret
auth_credentials = "BlueCats " + base64.b64encode(raw_auth)
```

The final credentials should be formatted as: <b>Bluecats {Base64EncodedCredentials}</b>

# API Endpoints 

This section contains outlines for API Endpoints and JSON structures with examples using [cURL](https://github.com/curl/curl).


## GET Resources
For getting resource information, you will usually supply a base_uri, a resource, and that resource ID with parameters if needed. An example of resources that you can get are: 

- Teams
- Sites 
- Beacons
- Beacon Settings
- Devices


| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | GET |
| *header*      | Authorization: {credentials}, Content-Type: application/json,  X-Api-Version: 3 |
| *path*   | {base_uri}/{resourceURL}  *See Below*|


| Resource Type | Resource URL |
|---------------|----------------------------------|
| *Teams*      	| {base_uri}/teams/{teamID} |
| *Sites*        | {base_uri}/sites/{siteID} |
| *Beacons*      | {base_uri}/beacons/{beaconID} |
| *Beacon Modes* | {base_uri}/beacons/{beaconID}/beaconmodes |
| *Beacon Target Speeds* | {base_uri}/beacons/{beaconID}/targetspeeds |
| *Beacon Loudnesses* | {base_uri}/beacons/{beaconID}/beaconloudnesses |
| *Beacon Regions*  | {base_uri}/beaconRegions/{regionID}|
| *Devices*      | {base_uri}/devices/{deviceID} |
| *Maps*      	   | {base_uri}/sites/{siteID}/Maps |
| *Packs* 		   | {base_uri}/packs/{claimcode} |


This Example GET Request pulls down all of the information for one Beacon. 
Similarly, you GET other resources like Devices, sites, and packs. 

<details>
	<summary> <b>Example GET Beacon Request</b> </summary>

	<pre><code>
	curl \
	-H "Authorization: Bluecats {Base64EncodedCredentials}" \
	-H "Content-Type: application/json" \
	-H "X-Api-Version: 3" \
	-X GET 'https://api.bluecats.com/beacons/{beaconSN}'
	</code></pre>
	
</details>

<details>
	<summary> <b>Example GET Beacon Response</b> </summary>

	<pre><code>
	{
	    "beacon": {
	        "name": "new",
	        "categories": [],
	        "customValues": [],
	        "iBeaconKey": "61687109-905F-4436-91F8-E602F514C96D:4:23326",
	        "siteName": "Newborn Beacon Incubator",
	        "pendingVersion": 44,
	        "batteryStatus": {
	            "name": "Dead",
	            "displayName": "Dead",
	            "id": 3
	        },
	        "healthStatus": {
	            "name": "NeverReported",
	            "displayName": "Never Reported",
	            "id": 4
	        },
	        "beaconLoudness": {
	            "name": "Talk",
	            "displayName": "Talk",
	            "level": 4,
	            "measuredPowerAt1Meter": -67,
	            "ranges": [],
	            "id": 28
	        },
	        "targetSpeed": {
	            "name": "Walk",
	            "displayName": "Walk",
	            "milliseconds": 319,
	            "id": 3
	        },
	        "measuredPowerAt1Meter": -67,
	        "createdAt": "2017-09-05T17:40:12Z",
	        "modifiedAt": "2017-11-23T16:28:28Z",
	        "firmwareVersion": "2.2.0",
	        "featureBitmask": 24810495,
	        "beaconRegion": {
	            "name": "BlueCats",
	            "regionIdentifier": "com.bluecats.BlueCats",
	            "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	            "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
	            "beaconCount": 1791,
	            "proximityUUIDAccessType": {
	                "name": "Public",
	                "displayName": "Public",
	                "id": 1
	            },
	            "namespaceIDAccessType": {
	                "name": "Public",
	                "displayName": "Public",
	                "id": 1
	            },
	            "namespaceID": "61687109E602F514C96D",
	            "monitorOnly": false,
	            "keepAliveProximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	            "keepAliveMajor": 0,
	            "keepAliveMinor": 0,
	            "id": "e1c67101-2fa7-fb8c-7142-1fe3067f69df"
	        },
	        "beaconMode": {
	            "name": "Custom",
	            "displayName": "Custom",
	            "id": 8
	        },
	        "modelNumber": "BC413",
	        "upgradableOTA": true,
	        "lastKnownBatteryLevel": 0,
	        "privacyDuration": 0,
	        "eddystone": {
	            "uid": "61687109E602F514C96D000000021EE6",
	            "url": "https://goo.gl/jaLfW3",
	            "namespaceID": "61687109E602F514C96D",
	            "instanceID": "000000021EE6",
	            "encodedUrl": "Z29vLmdsL2phTGZXMw==",
	            "urlSchemePrefixID": 3
	        },
	        "lastVisitedAt": "2018-01-31T21:11:29Z",
	        "securityType": {
	            "name": "TripleKey",
	            "displayName": "TripleKey",
	            "id": 3
	        },
	        "wireframeUrl": "http://res.cloudinary.com/hehoijro7/image/upload/v1469032691/bn1jph5sxzqwa3tnhnjk.png",
	        "connectable": true,
	        "eddystoneService": false,
	        "version": 35,
	        "latestFirmwareVersion": "2.4.0",
	        "firmwareUID": "5000006E",
	        "latestFirmwareUID": "5000008F",
	        "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	        "major": 4,
	        "minor": 23326,
	        "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
	        "siteID": "c0357201-2fa7-7a9e-1e44-2ba7323905eb",
	        "serialNumber": "50002A56",
	        "bluetoothAddress": "98072D067D27",
	        "id": "3f322301-e5a7-e1bf-2443-11c71c2b073f"
	    }
	}
	</code></pre>
	
</details>


 
## PUT Resources

In order to edit resources, you can PUT to a resources endpoint. To edit beacons or devices, you can PUT settings with a body of JSON. 

There are a few settings that require to GET the available information for that object. For example, to patch the setting Beacon Loudness to a Beacon, you will have to first GET all of the available Beacon Loudness IDs for that Beacon. After you get the corresponding Beacon Loudness ID, you can patch it to the API. 

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | PUT |
| *header*      | Authorization: {credentials}, Content-Type: application/json,  X-Api-Version: 3 |
| *path*        | {base_uri}/beacons/{beaconID} OR  {base_uri}/devices/{deviceID} |
| *body*        | *See Editable Beacon Settings Below*|


For an Example PUT Request, firstly GET a Beacon's/Device's (resource's) information.

Then you should edit the fields that you want. The possible [editable Beacon Settings](https://bluecats.github.io/documentation/dman/BlueCats-API#editable-beacon-settings) are below. Any settings with the suffix -ID, will require you to GET that resources ID. For example, you will get the  {base_uri}/beacons/{beaconID}/beaconmodes so that you can give the BlueCats API the proper ID back. Save that response to a JSON file.

PUT to the resource with those change. Similarly to the GET, you PUT other resources like Devices and Beacons. 

<details>
	<summary> <b>Example PUT Request</b> </summary>

	<pre><code>
	curl \
	-H "Authorization: Bluecats {Base64EncodedCredentials}" \
	-H "Content-Type: application/json" \
	-H "X-Api-Version: 3" \
	-X PUT --data '@beacon.json' 'https://api.bluecats.com/beacons/{beaconSN}'
	</code></pre>
	
	
</details>

<details>
	<summary> <b>Example PUT beacon.json BODY</b> </summary>

	<pre><code>
		{
		  "beacon": {
		    "name": "Beacon1",
		    "categories": [
		      
		    ],
		    "customValues": [
		      
		    ],
		    "iBeaconKey": "36B884FE-685B-46FA-A98B-F004D7F41048:5000:1",
		    "siteName": "Newborn Beacon Incubator",
		    "pendingVersion": 44,
		    "batteryStatus": {
		      "name": "Dead",
		      "displayName": "Dead",
		      "id": 3
		    },
		    "healthStatus": {
		      "name": "NeverReported",
		      "displayName": "Never Reported",
		      "id": 4
		    },
		    "beaconLoudness": {
		      "name": "Talk",
		      "displayName": "Talk",
		      "level": 4,
		      "measuredPowerAt1Meter": -67,
		      "ranges": [
		        
		      ],
		      "id": 28
		    },
		    "targetSpeed": {
		      "name": "Walk",
		      "displayName": "Walk",
		      "milliseconds": 319,
		      "id": 3
		    },
		    "measuredPowerAt1Meter": -67,
		    "createdAt": "2017-09-05T17:40:12Z",
		    "modifiedAt": "2018-05-10T19:27:42.5088353Z",
		    "firmwareVersion": "2.2.0",
		    "featureBitmask": 24810495,
		    "beaconRegion": {
		      "name": "dmanTest",
		      "regionIdentifier": "com.bluecats.dmanTest",
		      "proximityUUID": "36B884FE-685B-46FA-A98B-F004D7F41048",
		      "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
		      "beaconCount": 0,
		      "proximityUUIDAccessType": {
		        "name": "Private",
		        "displayName": "Private",
		        "id": 2
		      },
		      "monitorOnly": false,
		      "keepAliveProximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
		      "keepAliveMajor": 0,
		      "keepAliveMinor": 0,
		      "id": "74293a01-23a8-0b80-c942-b88f3a3bea77"
		    },
		    "beaconMode": {
		      "name": "Custom",
		      "displayName": "Custom",
		      "id": 8
		    },
		    "modelNumber": "BC413",
		    "upgradableOTA": true,
		    "lastKnownBatteryLevel": 0,
		    "privacyDuration": 0,
		    "eddystone": {
		      "url": "https:\/\/goo.gl\/jaLfW3",
		      "encodedUrl": "Z29vLmdsL2phTGZXMw==",
		      "urlSchemePrefixID": 3
		    },
		    "lastVisitedAt": "2018-01-31T21:11:29Z",
		    "securityType": {
		      "name": "TripleKey",
		      "displayName": "TripleKey",
		      "id": 3
		    },
		    "wireframeUrl": "http:\/\/res.cloudinary.com\/hehoijro7\/image\/upload\/v1469032691\/bn1jph5sxzqwa3tnhnjk.png",
		    "connectable": true,
		    "eddystoneService": false,
		    "version": 44,
		    "latestFirmwareVersion": "2.4.0",
		    "firmwareUID": "5000006E",
		    "latestFirmwareUID": "5000008F",
		    "proximityUUID": "36B884FE-685B-46FA-A98B-F004D7F41048",
		    "major": 5000,
		    "minor": 1,
		    "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
		    "siteID": "c0357201-2fa7-7a9e-1e44-2ba7323905eb",
		    "serialNumber": "50002A56",
		    "bluetoothAddress": "98072D067D27",
		    "id": "3f322301-e5a7-e1bf-2443-11c71c2b073f"
		  }
		}
	
	</code></pre>
	
</details>



<details>
	<summary> <b>Example PUT Response</b> </summary>

	<pre><code>
	{
	    "beacon": {
	        "name": "new",
	        "categories": [],
	        "customValues": [],
	        "iBeaconKey": "61687109-905F-4436-91F8-E602F514C96D:4:23326",
	        "siteName": "Newborn Beacon Incubator",
	        "pendingVersion": 44,
	        "batteryStatus": {
	            "name": "Dead",
	            "displayName": "Dead",
	            "id": 3
	        },
	        "healthStatus": {
	            "name": "NeverReported",
	            "displayName": "Never Reported",
	            "id": 4
	        },
	        "beaconLoudness": {
	            "name": "Talk",
	            "displayName": "Talk",
	            "level": 4,
	            "measuredPowerAt1Meter": -67,
	            "ranges": [],
	            "id": 28
	        },
	        "targetSpeed": {
	            "name": "Walk",
	            "displayName": "Walk",
	            "milliseconds": 319,
	            "id": 3
	        },
	        "measuredPowerAt1Meter": -67,
	        "createdAt": "2017-09-05T17:40:12Z",
	        "modifiedAt": "2017-11-23T16:28:28Z",
	        "firmwareVersion": "2.2.0",
	        "featureBitmask": 24810495,
	        "beaconRegion": {
	            "name": "BlueCats",
	            "regionIdentifier": "com.bluecats.BlueCats",
	            "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	            "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
	            "beaconCount": 1791,
	            "proximityUUIDAccessType": {
	                "name": "Public",
	                "displayName": "Public",
	                "id": 1
	            },
	            "namespaceIDAccessType": {
	                "name": "Public",
	                "displayName": "Public",
	                "id": 1
	            },
	            "namespaceID": "61687109E602F514C96D",
	            "monitorOnly": false,
	            "keepAliveProximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	            "keepAliveMajor": 0,
	            "keepAliveMinor": 0,
	            "id": "e1c67101-2fa7-fb8c-7142-1fe3067f69df"
	        },
	        "beaconMode": {
	            "name": "Custom",
	            "displayName": "Custom",
	            "id": 8
	        },
	        "modelNumber": "BC413",
	        "upgradableOTA": true,
	        "lastKnownBatteryLevel": 0,
	        "privacyDuration": 0,
	        "eddystone": {
	            "uid": "61687109E602F514C96D000000021EE6",
	            "url": "https://goo.gl/jaLfW3",
	            "namespaceID": "61687109E602F514C96D",
	            "instanceID": "000000021EE6",
	            "encodedUrl": "Z29vLmdsL2phTGZXMw==",
	            "urlSchemePrefixID": 3
	        },
	        "lastVisitedAt": "2018-01-31T21:11:29Z",
	        "securityType": {
	            "name": "TripleKey",
	            "displayName": "TripleKey",
	            "id": 3
	        },
	        "wireframeUrl": "http://res.cloudinary.com/hehoijro7/image/upload/v1469032691/bn1jph5sxzqwa3tnhnjk.png",
	        "connectable": true,
	        "eddystoneService": false,
	        "version": 35,
	        "latestFirmwareVersion": "2.4.0",
	        "firmwareUID": "5000006E",
	        "latestFirmwareUID": "5000008F",
	        "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
	        "major": 4,
	        "minor": 23326,
	        "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
	        "siteID": "c0357201-2fa7-7a9e-1e44-2ba7323905eb",
	        "serialNumber": "50002A56",
	        "bluetoothAddress": "98072D067D27",
	        "id": "3f322301-e5a7-e1bf-2443-11c71c2b073f"
	    }
	}
	</code></pre>
	
</details>


## PATCH Resources

In order to edit resources, you can PATCH to a resources endpoint. To edit beacons or devices, you can PATCH settings with a body of JSON. 

There are a few settings that require to GET the available information for that object. For example, to patch the setting Beacon Loudness to a Beacon, you will have to first GET all of the available Beacon Loudness IDs for that Beacon. After you get the corresponding Beacon Loudness ID, you can patch it to the API. 

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | PATCH |
| *header*      | Authorization: {credentials}, Content-Type: application/json,  X-Api-Version: 3 |
| *path*        | {base_uri}/beacons/{beaconID} OR  {base_uri}/devices/{deviceID} |
| *body*        | *See Editable Beacon Settings Below*|


For an Example PATCH Request, firstly GET a Beacon's/Device's (resource's) information, so you know what you can change. 

Then send only the possible [editable Beacon settings](https://bluecats.github.io/documentation/dman/BlueCats-API#editable-beacon-settings) as a JSON object. Any settings with the suffix -ID, will require you to GET that resources ID. For example, you will get the {base_uri}/beacons/{beaconID}/beaconmodes so that you can give the BlueCats API the proper ID back. You only PATCH the key:value pairs that you want from the editable Beacon settings. 

PATCH to the resource with those change. Similarly to the GET, you PATCH other resources like Devices and Beacons. 


<details>
	<summary> <b>Example Beacon PATCH Request</b> </summary>

	<pre><code>
	
	curl \
	-H "Authorization: Bluecats {Base64EncodedCredentials}" \
	-H "Content-Type: application/json" \
	-H "X-Api-Version: 3" \
	-X PATCH --data '{"name": "Beacon1"}' 'https://api.bluecats.com/beacons/{beaconSN}'
	
	</code></pre>
</details>

<details>
	<summary> <b>Example PATCH Response</b> </summary>

	<pre><code>
		{
		  "beacon": {
		    "name": "Beacon1",
		    "categories": [
		      
		    ],
		    "customValues": [
		      
		    ],
		    "iBeaconKey": "36B884FE-685B-46FA-A98B-F004D7F41048:5000:1",
		    "siteName": "Newborn Beacon Incubator",
		    "pendingVersion": 44,
		    "batteryStatus": {
		      "name": "Dead",
		      "displayName": "Dead",
		      "id": 3
		    },
		    "healthStatus": {
		      "name": "NeverReported",
		      "displayName": "Never Reported",
		      "id": 4
		    },
		    "beaconLoudness": {
		      "name": "Talk",
		      "displayName": "Talk",
		      "level": 4,
		      "measuredPowerAt1Meter": -67,
		      "ranges": [
		        
		      ],
		      "id": 28
		    },
		    "targetSpeed": {
		      "name": "Walk",
		      "displayName": "Walk",
		      "milliseconds": 319,
		      "id": 3
		    },
		    "measuredPowerAt1Meter": -67,
		    "createdAt": "2017-09-05T17:40:12Z",
		    "modifiedAt": "2018-05-10T19:27:42.5088353Z",
		    "firmwareVersion": "2.2.0",
		    "featureBitmask": 24810495,
		    "beaconRegion": {
		      "name": "dmanTest",
		      "regionIdentifier": "com.bluecats.dmanTest",
		      "proximityUUID": "36B884FE-685B-46FA-A98B-F004D7F41048",
		      "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
		      "beaconCount": 0,
		      "proximityUUIDAccessType": {
		        "name": "Private",
		        "displayName": "Private",
		        "id": 2
		      },
		      "monitorOnly": false,
		      "keepAliveProximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
		      "keepAliveMajor": 0,
		      "keepAliveMinor": 0,
		      "id": "74293a01-23a8-0b80-c942-b88f3a3bea77"
		    },
		    "beaconMode": {
		      "name": "Custom",
		      "displayName": "Custom",
		      "id": 8
		    },
		    "modelNumber": "BC413",
		    "upgradableOTA": true,
		    "lastKnownBatteryLevel": 0,
		    "privacyDuration": 0,
		    "eddystone": {
		      "url": "https:\/\/goo.gl\/jaLfW3",
		      "encodedUrl": "Z29vLmdsL2phTGZXMw==",
		      "urlSchemePrefixID": 3
		    },
		    "lastVisitedAt": "2018-01-31T21:11:29Z",
		    "securityType": {
		      "name": "TripleKey",
		      "displayName": "TripleKey",
		      "id": 3
		    },
		    "wireframeUrl": "http:\/\/res.cloudinary.com\/hehoijro7\/image\/upload\/v1469032691\/bn1jph5sxzqwa3tnhnjk.png",
		    "connectable": true,
		    "eddystoneService": false,
		    "version": 44,
		    "latestFirmwareVersion": "2.4.0",
		    "firmwareUID": "5000006E",
		    "latestFirmwareUID": "5000008F",
		    "proximityUUID": "36B884FE-685B-46FA-A98B-F004D7F41048",
		    "major": 5000,
		    "minor": 1,
		    "teamID": "dfc67101-2fa7-4c85-2647-c3ae556d95ef",
		    "siteID": "c0357201-2fa7-7a9e-1e44-2ba7323905eb",
		    "serialNumber": "50002A56",
		    "bluetoothAddress": "98072D067D27",
		    "id": "3f322301-e5a7-e1bf-2443-11c71c2b073f"
		  }
		}
	
	</code></pre>
	
</details>



## Editable Beacon Settings 

These are decriptions of Settings that are editable through PATCHing or PUTing a Beacon. If you do not know the setting you are changing it to, you should try to GET that resource first. 

|Beacon Setting Name| Description | Body Example |
|-------------------|-------------|--------------| 
| "name" | name of the Beacon | {"name": "Beacon1"}|
| "siteID" | siteID | {"siteID": "{siteID}"}|
| "targetSpeedID" | targetSpeedID | {"targetSpeedID": "{targetSpeedID}"}|
| "beaconLoudnessID" | beaconLoudnessID | {"beaconLoudnessID": "{beaconLoudnessID}"} | 
| "beaconModeID" | beaconModeID | {"beaconModeID": "{beaconModeID}"} | 
| "mapID" | MapID | {"mapID": "{mapID}"} |
| "mapX" | MapX | {"mapX": "{mapX}"} |
| "mapY" | MapY | {"mapY": "{mapY}"} |
| "categoryList" | categoryList |{"categoryList": ["category1", "category2"]}|
| "customValueList" | customValueList |{"customValueList": [{"key": "key", "value": "value"}]}|
| "privacyDuration" | privacyDuration in minutes. Set to 0 to turn it off  | {"privacyDuration": 1440}|
| "notes" | any notes you want beacon to have |{"notes": "{notes}"}|

