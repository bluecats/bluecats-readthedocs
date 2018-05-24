---
layout: documentation
title: Loop API 0.2.1
---


This describes the resources for the Loop API. If you have any problems or requests contact [support@bluecats.com](mailto:support@bluecats.com). 

## Introduction 


The Loop API allows you to: 

- Create or update a single or multiple (batch) Groups or Objects
- Change the password of the Group
<!--- - Delete a Group or an Object --->
- Claim or unclaim a single or multiple (batch) Objects by the Owner
- Send batch Events 
- Search and perform analytics on Groups, Objects, and Events

## Connecting to the API 

| Environment   | Connectivity | PORT |
|---------------|--------------|------|
| *Labs*        | URL: https://bcstage.api.loop.bluecats.com/ | 443 (default https port)|
| *Production*  | URL: {base_uri} |443 (default https port)|

# Endpoints and JSON
This section contains outlines for API Endpoints and JSON structures with examples using [cURL](https://github.com/curl/curl).

## Login
Login credentials are returned after a GET request for user login. *Basic Authorization Credentials* is returned after a GET request for user login. Adding the *Basic Authorization Credentials* to your header allows you to make requests POST/GET groups. 

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | GET |
| *path*        | {base_uri}/login?email={email}&password={password} |


**Request** 

```ruby
curl \
-X GET 'https://bcstage.api.loop.bluecats.com/login?'\
	'email={email}&'\
	'password={password}'
```

**Response** 

```ruby
{
	"auth": "Basic {Authorization Token}",
	"groupID": "764166c0c49c4e113c62f0ca3cecd84e", 
	"email": "{email}",
	"firstName": "{firstName}", 
	"lastName": "{lastName}"
}
``` 


## Groups Query
Performing a GET request on *groups* allows you to see all of the *groups* that you are authorized to access. Querying groups also allows you to determine *Groups IDs*. *Group IDs* allow you to create objects in that groups and perform other meaningful queries. 

| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | GET |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/groups|


**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-X GET 'https://bcstage.api.loop.bluecats.com/groups'
```

**Response** 

```ruby

{
  "id": "764166c0c49c4e113c62f0ca3cecd84e",
  "type": "Root",
  "name": "BlueCats",
  "abbreviation": "bc",
  "children": {
    "id": "a3cec63e2aa927c93567abd968677314",
    "type": "Tenant",
    "name": "BlueCats Demo",
    "abbreviation": "bcd",
    "children": [
      {
        "id": "5390eea486cad80464c24ca2e861837a",
        "type": "Customer",
        "name": "BC Car Park SYD",
        "abbreviation": "bcdcparksyd"
      },
      {
        "id": "66679cbce97e147f3ef61122eb63a1a3",
        "type": "Customer",
        "name": "BC Local Fulfillment ATX",
        "abbreviation": "bcdlocfulatx"
      },
      {
        "id": "9d6d3255547edffe380502e554bfb24d",
        "type": "Customer",
        "name": "BC Internal Stock Tranfers",
        "abbreviation": "bcdintstktrans"
      },
      {
        "id": "d3f78b1267b5bd9f3531009fea8365d",
        "type": "Customer",
        "name": "BC Car Lot ATX",
        "abbreviation": "bcdclotatx"
      }
    ]
  }
}

```

## ObjectTypes Query 

Performing a GET request on *objectTypes* is useful to determine which objects are available to which groups and which you can query. The group ID in the objectTypes shows what group created and has ownership of the objectType. Anyone underneath that group can create objects. When creating objects, check which object keys are required and what types they are when you want to create objects. 


| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | GET |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/objectTypes|


**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-X GET 'https://bcdev.api.loop.bluecats.com/schema'
```

**Response** 

```ruby

  {
    "name": "bcdEquipment",
    "displayName": "Equipment",
    "class": "Thing",
    "keys": [
      {
        "name": "type",
        "displayName": "Type",
        "required": true,
        "type": "varchar(100)",
        "editable": true
      },
      {
        "name": "serialNumber",
        "displayName": "Serial Number",
        "required": false,
        "type": "varchar(100)",
        "editable": true
      },
      {
        "name": "batterySoC",
        "displayName": "Battery %",
        "required": false,
        "type": "integer",
        "editable": false
      },
      {
        "name": "temp",
        "displayName": "Temp",
        "required": false,
        "type": "varchar(10)",
        "editable": false
      },
      {
        "name": "tempObservedAt",
        "displayName": "Observed At",
        "required": false,
        "type": "datetime(6)",
        "editable": false
      },
      {
        "name": "place",
        "displayName": "Place",
        "required": false,
        "type": "varchar(32)",
        "objectLink": {
          "table": "Place",
          "nested": true
        },
        "editable": false
      },
      {
        "name": "placeChangedAt",
        "displayName": "Changed At",
        "required": false,
        "type": "datetime(6)",
        "editable": false
      },
      {
        "name": "long",
        "displayName": "Longitude",
        "required": false,
        "type": "decimal(13,10)",
        "editable": false
      },
      {
        "name": "lat",
        "displayName": "Latitude",
        "required": false,
        "type": "decimal(13,10)",
        "editable": false
      },
      {
        "name": "positionObservedAt",
        "displayName": "Observed At",
        "required": false,
        "type": "datetime(6)",
        "editable": false
      },
      {
        "name": "accObservedAt",
        "displayName": "Observed At",
        "required": false,
        "type": "datetime(6)",
        "editable": false
      },
      {
        "name": "eventID",
        "displayName": "Event ID",
        "required": false,
        "type": "varchar(100)",
        "editable": true,
        "hidden": true
      }
    ],
    "groupID": "a3cec63e2aa927c93567abd968677314"
  }
```

## Create Object
Login credentials are returned after a GET request for user login.

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | POST |
| *header*      | Authorization: Basic {credentials}, Content-Type: application/json|
| *path* (req)  | {base_uri}/objects?objectType={objectType} |
| *body*        | { "groupID": {groupID}, "requiredKey": {requiredValue}, "optionalKey": {optionalValue} }|

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X POST --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects?objectType=Place'
```

**Response** 

```ruby
"success"
```

## Objects Query
Login credentials are returned after a GET request for user login.

| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | GET |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/objects|


## Query Params 

| **Key**       | **Value** | Required | Explanation | Example |
|---------------|-----------|----------|-------------|---------|
| objectType | *string*| Yes| specifying objectType to query|objectType=bcdPerson|
| limit | *int*  | No | Number of objects returned (max 200)| limit=34 |
| first | *int*  | No | Index of object to start from (when paging) |first=67|
| sortkey | *string* | No | Sorting objects by a key (defaults to ascending) | sortkey=name|
| sortdir |*string* | No | Sorting objects by a key and direction ascending or descending (asc or desc)| sortkey=name&sortdir=desc|
| ssearch| *string* | No | Index of object to start from, when paging |ssearch=ernie |


**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-X GET 'https://bcstage.api.loop.bluecats.com/objects?'\
	'objectType=bcdPerson'\
```

**Response** 

```ruby

{
	"objects": 
	{
		"id": "439859f6c79bf12eab0e338bc3194a7b", 
		"groupID": "66679cbce97e147f3ef61122eb63a1a3", 
		"eventID": "eddyUID#3EBC2C6462A6D9E7BF47000000000005", 
		"name": "Ernie Forzano", 	
		"serialNumber": null,
		"batterySoC": 65, 
		"batterySoCObservedAt": "2017-10-24T14:59:10.982710Z", 
		"temp": null, "tempObservedAt": null, "impactMagnitude": null,
		"impactObservedAt": null, 
		"place": 
		{
			"id": "4d27ec3d975b45f9af2137a240afcf9b", 
			"groupID": "66679cbce97e147f3ef61122eb63a1a3", 
			"eventID": null, 
			"name": "BC ATX Office", 
			"fullName": "BlueCats Austin, Texas Office", 
			"countryCode": "US", 
			"adminCode": "TX", 
			"city": "Austin", 
			"postalCode": "78702", 
			"street": "301 Chicon, Suite A", 
			"boundingBox": 
				"{\"type\":\"Polygon\",\"coordinates\":			[[-97.72360056638718,30.25915060702415],[-97.72323042154312,30.259018550957144],[-97.72332161664963,30.2588366839777],[-97.72368639707565,30.258971057065057],[-97.72360056638718,30.25915060702415]]}", 
			"attributes": 
				"{\"fillColor\":\"#FF0000\",
				\"fillOpacity\":0.35,
				\"strokeColor\":\"#FF0000\",
				\"strokeWeight\":2,
				\"strokeOpacity\":0.8}"
		}, 
		"placeChangedAt": "2017-10-24T14:36:30.849691Z", 
		"long": -97.7234845608, 
		"lat": 30.259081683, 
		"positionObservedAt": "2017-10-24T14:36:30.849691Z", 		"objectType": "bcdPerson"
	},  

	{
		"id": "b5d3daebcad1bef1ff660d13ec80a1df", 
		"groupID": "66679cbce97e147f3ef61122eb63a1a3", "eventID": 		"eddyUID#3EBC2C6462A6D9E7BF47000000000003", 
		"name": "Jarran Pedersen", 
		"serialNumber": null, 
		"batterySoC": 83, 
		"batterySoCObservedAt": "2017-10-23T22:57:47.838706Z", 
		"temp": null, 
		"tempObservedAt": null, 
		"impactMagnitude": null, 
		"impactObservedAt": null, 
		"place": null, 
		"placeChangedAt": "2017-10-23T22:58:08.279756Z", 
		"long": null, 
		"lat": null, 
		"positionObservedAt": "2017-10-23T22:58:08.279756Z", 
		"objectType": "bcdPerson"}], 
"start": 0, "count": 2, "total": 2
}

```


# Query Events 

Login credentials are returned after a GET request for user login.
Any event type located here. Those event types will be saved. However, it will not necessarily have state logic applied to it. 

| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | GET |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/events|


## Event Query Params 

| **Key**  | **Value** | Required | Explanation | Examples |
|----------|-----------|----------|----------|---------|
| identifier | *string*| Yes| specifying identifier to query|identifier={identifier} check below|
| eventType | *string* | No | [event types]({{ 'documentation/loop/bc-loop-event-types' | relative_url }}) | eventType |
| lastKeyID | *string* | No | Starts from the event after referenced. (*Requires* lastKeyTS) | "lastKeyID": "bcId#50002D7D", "lastKeyTS": "2017-11-01T17:34:59.801528Z"|
| lastKeyTS | *string* | No | Starts from the event after referenced. (*Requires* lastKeyID) | "lastKeyID": "bcId#50002D7D", "lastKeyTS": "2017-11-01T17:34:59.801528Z"|
| count | *int*  | No | Number of objects returned (max 100)| count=34 |


### Event Identifiers Examples 

- identifier=edge#{edgeMAC}
- identifier=ibeac#{ibeaconkeyinhex}
- identifier=eddyUID#{eddystoneUID}
- identifier=bcId#{bc custom identifier, normally serial number}
- identifier=btAddr#{bluetooth address}

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-X GET 'https://bcstage.api.loop.bluecats.com/events?'\
	'identifier=bcId%2350002D7D'
```

**Response** 

```ruby

{
  "events": [
    {
      "ts": "2017-11-01T17:35:33.382555Z",
      "bcId": "50002D7D",
      "event": "enZone",
      "edgeMAC": "E4956E4E4024",
      "identifier": "bcId#50002D7D",
      "mac": "6A35258BB9E5",
      "enZone": "CALIFORNIA",
      "identifierAndType": "bcId#50002D7D#enZone"
    },
    {
      "ts": "2017-11-01T17:35:25.185150Z",
      "bcId": "50002D7D",
      "event": "exZone",
      "edgeMAC": "E4956E4E4024",
      "exZone": "CALIFORNIA",
      "identifier": "bcId#50002D7D",
      "mac": "6A35258BB9E5",
      "identifierAndType": "bcId#50002D7D#exZone"
    },
    {
      "ts": "2017-11-01T17:35:12.991150Z",
      "bcId": "50002D7D",
      "event": "enZone",
      "edgeMAC": "E4956E4E4024",
      "identifier": "bcId#50002D7D",
      "mac": "6A35258BB9E5",
      "enZone": "CALIFORNIA",
      "identifierAndType": "bcId#50002D7D#enZone"
    },
    {
      "ts": "2017-11-01T17:35:11.589368Z",
      "bcId": "50002D7D",
      "event": "exZone",
      "edgeMAC": "E4956E4E4024",
      "exZone": "CALIFORNIA",
      "identifier": "bcId#50002D7D",
      "mac": "6A35258BB9E5",
      "identifierAndType": "bcId#50002D7D#exZone"
    },
    {
      "ts": "2017-11-01T17:34:59.801528Z",
      "bcId": "50002D7D",
      "event": "enZone",
      "edgeMAC": "E4956E4E4024",
      "identifier": "bcId#50002D7D",
      "mac": "6A35258BB9E5",
      "enZone": "CALIFORNIA",
      "identifierAndType": "bcId#50002D7D#enZone"
    }
  ],
  "count": 5,
  "lastKeyID": "bcId#50002D7D",
  "lastKeyTS": "2017-11-01T17:34:59.801528Z"
}

```


# Posting Events 

Login credentials are returned after a GET request for user login.

| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | POST |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/events|
| *body*        | {event body} | 


### Event Body 

In general for the Event Body, you will have a edgeMac identifier this is where the Events are being posted from. 

```
{
	"edgeMac":{identifier} 

```

If you have GPS values, you can have a location dictionary with key value pairs for lat and long. Including values for Lat and long will create a an Edge Location Event. For the locations lat and long, the value can be empty "" or contain a {value}. 

```
	"loc": {  
      "lat": "" or {value},
      "long": "" or {value},
   }
```
   
Nested underneath will be an events list. *Time Stamp*, an *identifier*, and *eventType* is required in each event. 

1. Time stamp is required in this UTC ISO-format: DATE:TIME (2017-07-20T18:42:34.129120Z). 

2. Identifiers are required and can be delivered in two formats:  
	- The edge relay format for: 
		- iBeac
		- eddyUID
		- bcId
		- btAddr
	- Identifier key value pair:
		- identifier=ibeac#{ibeaconkeyinhex}
		- identifier=eddyUID#{eddystoneUID}
		- identifier=bcId#{bc custom identifier, normally serial number}
		- identifier=btAddr#{bluetooth address}
		- identifier=Whatever {though this does not mean it will be processed by State Logic}

3. The final thing required for this eventType. There are many different [event types]({{ '/documentation/loop/bc-loop-event-types'| relative_url }}). There are other things that can be included in the events. It will be saved but does not necessarily mean it will be processed. 

```
   "events":[  
      {  
         "mac":"D42202000053",
         "ts":"2017-07-20T18:42:34.129120Z",
         "event":"exZone",
         "exZone":"IN",
         "iBeac":"E2C56DB5DFFB48D2B060D0F5A71096E000000000"
      }
   ]
 }  
```   
	
Here is a complete example that can be sent up as POST. 
There are many different iterations of how you can POST Events. Save an event.json file to POST. 

**Example (event.json)**

```

{  
   "edgeMAC":"E4956E4CC2E0",
   "loc":{  
      "lat":"",
      "long":"",
      "fixQual":0,
      "spd":"",
      "alt":""
   },
   "events":[  
      {  
         "mac":"D42202000053",
         "ts":"2017-07-20T18:42:34.129120Z",
         "event":"exZone",
         "exZone":"IN",
         "iBeac":"E2C56DB5DFFB48D2B060D0F5A71096E000000000"
      },
      {  
         "mac":"D42202000053",
         "ts":"2017-07-20T18:42:34.205799Z",
         "event":"enZone",
         "enZone":"ME",
         "iBeac":"E2C56DB5DFFB48D2B060D0F5A71096E000000000"
      },
      {  
         "mac":"98072D05F878",
         "ts":"2017-07-20T18:42:34.273086Z",
         "event":"enZone",
         "enZone":"ME"
      },
      {  
         "mac":"A0E6F854F546",
         "ts":"2017-07-20T18:42:34.816404Z",
         "event":"enZone",
         "enZone":"ME"
      },
      {  
         "mac":"44FB9726E7B1",
         "ts":"2017-07-20T18:42:35.437517Z",
         "event":"enZone",
         "enZone":"ME"
      },
      {  
         "mac":"98072D068212",
         "ts":"2017-07-20T18:42:36.254939Z",
         "event":"enZone",
         "enZone":"ME"
      }
   ]
}


```

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X POST --data "@event.json" 'https://bcstage.api.loop.bluecats.com/events?'
	
```

**Request**

```ruby

"Success: 3 events reported"
	
```


