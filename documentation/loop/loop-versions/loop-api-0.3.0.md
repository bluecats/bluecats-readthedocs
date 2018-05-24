---
layout: documentation
title: Loop API 0.3.0
---

This describes the resources for the Loop API. If you have any problems or requests contact [support@bluecats.com](mailto:support@bluecats.com). 


## New Features:

- Login is now a POST to receive credentials instead of GET. 
- GET The new Schema endpoint allows to search more information about what is avialble for Objects.
- PATCH or DELETE to the objects endpoint allows for editing and deleting objects is now availible. 
- POST JSON to the Search Endpoint which allows search resources with Clause dictionaries and Comparison Operators.

## Table of Contents:

- [Introduction](loop-api-0.3.0#introduction)
	- [Connecting to the API](loop-api-0.3.0#connecting-to-the-api)
- [API Endpoints](loop-api-0.3.0#api-endpoints)
	- [Login](loop-api-0.3.0#login) 
	- [Schema Query](loop-api-0.3.0#schema-query)
		- [Optional Query Params](loop-api-0.3.0#optional-query-params)
		- [Data Types](loop-api-0.3.0#data-types)
	- [Create Objects](loop-api-0.3.0#create-objects) 
	- [Edit Objects](loop-api-0.3.0#edit-objects) 
	- [Delete Objects](loop-api-0.3.0#delete-objects) 
	- [Event Query Params](loop-api-0.3.0#event-query-params)
		- [Event Identifiers Examples](loop-api-0.3.0#event-identifiers-examples)
	- [Posting Events](loop-api-0.3.0#posting-events)  
		- [Event Body](loop-api-0.3.0#event-body)  
	- [Search Endpoint](loop-api-0.3.0#search-endpoint)  
	- [Post Search](loop-api-0.3.0#post-search)  
	- [Search Body](loop-api-0.3.0#search-body)  
	- [Search Filters](loop-api-0.3.0#search-filters-where)  
		- [Clause Dictionary](loop-api-0.3.0#clause-dictionary) 
		- [Comparison Operation Available for Types](loop-api-0.3.0#comparison-operation-available-for-types)  
		- [Query List](loop-api-0.3.0#query-list)    

## Introduction 

The Loop API allows you to: 

- Create or update a single or multiple (batch) Groups or Objects
- Change the password of the Group
- Delete an Object 
- Claim or unclaim a single or multiple (batch) Objects by the Owner
- Send batch Events 
- Search and perform analytics on Groups, Objects, and Events

## Connecting to the API 

| Environment   | Connectivity | PORT |
|---------------|--------------|------|
| *Labs*        | URL: https://bcstage.api.loop.bluecats.com/ | 443 (default https port)|
| *Production*  | URL: {base_uri} |443 (default https port)|

# API Endpoints 
This section contains outlines for API Endpoints and JSON structures with examples using [cURL](https://github.com/curl/curl).

## Login
Login credentials are returned after a POST request with your email and password in the body as JSON for user login. *Basic Authorization Credentials* is returned after a POST request for user login. Adding the *Basic Authorization Credentials* to your header allows you to make requests POST/GET groups. 

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | POST |
| *path*        | {base_uri}/login |
| *body*        | { "email": {email}, "password": {password} }|

**Request** 

```ruby
curl \
-H "Content-Type: application/json" \
-X POST --data "@login.json" 'https://bcstage.api.loop.bluecats.com/login'
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

## Schema Query  

*Schema* replaces the object-types endpoint. Performing a GET request on *Schema* is useful to determine which objects are available to which groups and which you can query. The group ID in the objectTypes shows what group created and has ownership of the objectType. Anyone underneath that group can create objects. When creating objects, check which object keys are required and what types they are when you want to create objects. 


| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | GET |
| *header*      | Authorization: Basic {credentials}|
| *path*        | {base_uri}/schema|


### Schema Query Params 

| **Key**       | **Value** | Required | Explanation | Example |
|---------------|-----------|----------|-------------|---------|
| incUIAttr     | *bool*    | No       |Exposes hidden UI Attributes within objectTypes |incUIAttr=True|
| groupID       | *string*  | No       |String filters down objectTypes based on group access| groupID={groupID} | 
 
### Data Types
 
When you query the *Schema* endpoint, you can display types and key information for the resources for users, groups, objects, objectTypes.  Everything stored in the database are these six data types.


| Type          | Description     |  Example |
|---------------|-----------------|----------|
| *string*      | length of string field that defaults to size 100 |"firstName"|
| *datetime*    | ISO combined date in time for UTC format |2017-10-31T18:44:38.129120Z |
| *decimal*     | 9 digits on each side of the decimal point, if bigger than it will be truncated | 34.2338|
| *int*         | 32-bits | 56|
| *json*        | application/json format | {"email":{email}, "password":{password}}|
| *bool*        | Only accepts True or False | True |

For each object type, it has a list of keys.
Each key has a type, field, and attributes. The keys have attributes like being if it is required, if the type is editable, or if it has a linked to another object. 

```ruby 

{
    "name": "groupID",
    "displayName": "Group ID",
    "required": true,
    "type": "string",
    "length": 32,
    "editable": true,
    "link": "groups"
}

```
 
In our example below, we will query the *Schema* endpoint with incUIAttr=True. If we did not, then the only difference in our response would be missing the dictionary uiAttr. 
 
**Request** 

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-X GET 'https://bcstage.api.loop.bluecats.com/schema?incUIAttr=True'
```


**Response** 

```ruby

{
    "users": {
        "keys": [
            {
                "name": "email",
                "type": "string",
                "length": 100
            },
            {
                "name": "firstName",
                "type": "string",
                "length": 100
            },
            {
                "name": "lastName",
                "type": "string",
                "length": 100
            },
            {
                "name": "groupID",
                "type": "string",
                "length": 100
            }
        ]
    },
    "groups": {
        "keys": [
            {
                "name": "id",
                "type": "string",
                "length": 32
            },
            {
                "name": "type",
                "type": "string",
                "length": 100
            },
            {
                "name": "name",
                "type": "string",
                "length": 100
            },
            {
                "name": "parentID",
                "type": "string",
                "length": 32,
                "link": "groups"
            },
            {
                "name": "dmanTeamID",
                "type": "string",
                "length": 40
            },
            {
                "name": "abbreviation",
                "type": "string",
                "length": 100
            },
            {
                "name": "uiAttr",
                "type": "json"
            },
            {
                "name": "accountInfo",
                "type": "json"
            }
        ]
    },
    "objectTypes": [
        "bcdEquipment",
        "bcdParcel",
        "bcdPerson",
        "bcdTableFlag",
        "bcdVehicle",
        "dccEquipment",
        "dccPerson",
        "Place",
        "sbugECV",
        "wiShockGard"
    ],
    "objects": {
        "bcdEquipment": {
            "name": "bcdEquipment",
            "displayName": "Equipment",
            "class": "Thing",
            "keys": [
                {
                    "name": "id",
                    "displayName": "ID",
                    "required": false,
                    "type": "string",
                    "length": 32,
                    "editable": false
                },
                {
                    "name": "groupID",
                    "displayName": "Group ID",
                    "required": true,
                    "type": "string",
                    "length": 32,
                    "editable": true,
                    "link": "groups"
                },
                {
                    "name": "eventID",
                    "displayName": "Event ID",
                    "required": false,
                    "type": "string",
                    "length": 100,
                    "editable": true
                },
                {
                    "name": "objectType",
                    "displayName": "Object Type",
                    "required": true,
                    "type": "string",
                    "length": 100,
                    "editable": false
                },
                {
                    "name": "type",
                    "displayName": "Type",
                    "required": true,
                    "type": "string",
                    "editable": true,
                    "length": 100
                },
                {
                    "name": "serialNumber",
                    "displayName": "Serial Number",
                    "required": false,
                    "type": "string",
                    "editable": true,
                    "length": 100
                },
                {
                    "name": "batterySoC",
                    "displayName": "Battery %",
                    "required": false,
                    "type": "int",
                    "editable": false
                },
                {
                    "name": "batterySoCObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "temp",
                    "displayName": "Temp",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 10
                },
                {
                    "name": "tempObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "impactMagnitude",
                    "displayName": "Impact Magnitude",
                    "required": false,
                    "type": "int",
                    "editable": false
                },
                {
                    "name": "impactObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "place",
                    "displayName": "Place",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 32,
                    "link": "Place"
                },
                {
                    "name": "placeChangedAt",
                    "displayName": "Changed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "long",
                    "displayName": "Longitude",
                    "required": false,
                    "type": "decimal",
                    "editable": false
                },
                {
                    "name": "lat",
                    "displayName": "Latitude",
                    "required": false,
                    "type": "decimal",
                    "editable": false
                },
                {
                    "name": "positionObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "accX",
                    "displayName": "Tilt X",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 10
                },
                {
                    "name": "accY",
                    "displayName": "Tilt Y",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 10
                },
                {
                    "name": "accZ",
                    "displayName": "Tilt Z",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 10
                },
                {
                    "name": "accObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "eventID",
                    "displayName": "Event ID",
                    "required": false,
                    "type": "string",
                    "length": 100,
                    "editable": true,
                    "hidden": true
                }
            ],
            "groupID": "a3cec63e2aa927c93567abd968677314",
            "uiAttr": {
                "faIcon": "fa-star",
                "keyAttr": {
                    "type": {
                        "hidden": false
                    },
                    "serialNumber": {
                        "hidden": false
                    },
                    "batterySoC": {
                        "hidden": false
                    },
                    "batterySoCObservedAt": {
                        "hidden": true
                    },
                    "temp": {
                        "hidden": true
                    },
                    "tempObservedAt": {
                        "hidden": true
                    },
                    "impactMagnitude": {
                        "hidden": true
                    },
                    "impactObservedAt": {
                        "hidden": true
                    },
                    "place": {
                        "hidden": false
                    },
                    "placeChangedAt": {
                        "hidden": true
                    },
                    "long": {
                        "hidden": true
                    },
                    "lat": {
                        "hidden": true
                    },
                    "positionObservedAt": {
                        "hidden": true
                    },
                    "accX": {
                        "hidden": true
                    },
                    "accY": {
                        "hidden": true
                    },
                    "accZ": {
                        "hidden": true
                    },
                    "accObservedAt": {
                        "hidden": true
                    }
                }
            }
        },
        "bcdParcel": {
            "name": "bcdParcel",
            "displayName": "Parcel",
            "class": "Thing",
            "keys": [
                {
                    "name": "id",
                    "displayName": "ID",
                    "required": false,
                    "type": "string",
                    "length": 32,
                    "editable": false
                },
                {
                    "name": "groupID",
                    "displayName": "Group ID",
                    "required": true,
                    "type": "string",
                    "length": 32,
                    "editable": true,
                    "link": "groups"
                },
                {
                    "name": "eventID",
                    "displayName": "Event ID",
                    "required": false,
                    "type": "string",
                    "length": 100,
                    "editable": true
                },
                {
                    "name": "objectType",
                    "displayName": "Object Type",
                    "required": true,
                    "type": "string",
                    "length": 100,
                    "editable": false
                },
                {
                    "name": "trackingNumber",
                    "displayName": "Tracking #",
                    "required": false,
                    "type": "string",
                    "editable": true,
                    "length": 100
                },
                {
                    "name": "condition",
                    "displayName": "Condition",
                    "required": false,
                    "type": "string",
                    "editable": true,
                    "length": 100
                },
                {
                    "name": "batterySoC",
                    "displayName": "Battery %",
                    "required": false,
                    "type": "int",
                    "editable": false
                },
                {
                    "name": "batterySoCObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "temp",
                    "displayName": "Temp",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 10
                },
                {
                    "name": "tempObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "impactMagnitude",
                    "displayName": "Impact Magnitude",
                    "required": false,
                    "type": "int",
                    "editable": false
                },
                {
                    "name": "impactObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "place",
                    "displayName": "Place",
                    "required": false,
                    "type": "string",
                    "editable": false,
                    "length": 32,
                    "link": "Place"
                },
                {
                    "name": "placeChangedAt",
                    "displayName": "Changed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "long",
                    "displayName": "Longitude",
                    "required": false,
                    "type": "decimal",
                    "editable": false
                },
                {
                    "name": "lat",
                    "displayName": "Latitude",
                    "required": false,
                    "type": "decimal",
                    "editable": false
                },
                {
                    "name": "positionObservedAt",
                    "displayName": "Observed At",
                    "required": false,
                    "type": "datetime",
                    "editable": false
                },
                {
                    "name": "eventID",
                    "displayName": "Event ID",
                    "required": false,
                    "type": "string",
                    "length": 100,
                    "editable": true,
                    "hidden": true
                }
            ],
            "groupID": "a3cec63e2aa927c93567abd968677314",
            "uiAttr": {
                "faIcon": "fa-archive",
                "keyAttr": {
                    "trackingNumber": {
                        "hidden": false
                    },
                    "condition": {
                        "hidden": false
                    },
                    "batterySoC": {
                        "hidden": false
                    },
                    "batterySoCObservedAt": {
                        "hidden": true
                    },
                    "temp": {
                        "hidden": true
                    },
                    "tempObservedAt": {
                        "hidden": true
                    },
                    "impactMagnitude": {
                        "hidden": true
                    },
                    "impactObservedAt": {
                        "hidden": true
                    },
                    "place": {
                        "hidden": false
                    },
                    "placeChangedAt": {
                        "hidden": true
                    },
                    "long": {
                        "hidden": true
                    },
                    "lat": {
                        "hidden": true
                    },
                    "positionObservedAt": {
                        "hidden": true
                    }
                }
            }
        }
    }
}        

``` 


## Create Objects

In order to Create Objects, you will POST to the *Objects* endpoint. To create objects, you will POST body JSON with:

- objectType in body
- groupID
- required keys
- and all of the optional keys. 


| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | POST |
| *header*      | Authorization: Basic {credentials}, Content-Type: application/json|
| *path* (req)  | {base_uri}/objects |
| *body*        | { "groupID": {groupID}, "objectType": {objectType}, "requiredKey": {requiredValue}, "optionalKey": {optionalValue} }|

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X POST --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'
```

**Response** 

```ruby
"success"
```


## Edit Objects

In order to Edit Objects, you will PATCH to the *Objects* endpoint. To edit objects, you will PATCH body JSON with:
 
- objectID and object type in the body
- any keys you want to change
- You can change -> groupID if it is in your groups 


| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | PATCH |
| *header*      | Authorization: Basic {credentials}, Content-Type: application/json|
| *path* (req)  | {base_uri}/objects |
| *body*        | { "id": {id}, "objectType": {objectType}, "editableKey": {editableValue} }|

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X PATCH --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'
```

**Response** 

```ruby
"success"
```

## Delete Objects


In order to delete Objects, you will DELETE to the *Objects* endpoint. To edit objects, you will DELETE body JSON with:
 
- objectType 
- ID 


| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | DELETE |
| *header*      | Authorization: Basic {credentials}, Content-Type: application/json|
| *path* (req)  | {base_uri}/objects |
| *body*        | { "id": {id}, "objectType": {objectType}}|

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X DELETE --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'
```

**Response** 

```ruby
"success"
```


<!---
/dman/snLookup POST 
check if BC beacon is registered with the API and if its already linked to another object 
if it does than it has 
WAIT
--->

## Query Events 

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

| HTTPS Element | Request Details | 
|---------------|-----------------|
| *method*      | POST |
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
-H "Content-Type: application/json" \
-X POST --data "@event.json" 'https://bcstage.api.loop.bluecats.com/events'
	
```

**Request**

```ruby

"Success: 3 events reported"
	
```


# Search Endpoint

The search API for Loop can perform queries and analytics on users, objects, and groups. Our API is based off the [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer).

The following examples outlines for API Endpoints and JSON structures with examples using [cURL](https://github.com/curl/curl).

## POST search
A Search API request contains the following elements:

| HTTPS Element | Request Details |
|---------------|----------------------------------|
| *method*      | POST |
| *header*      | Authorization: Basic {credentials}, Content-Type: application/json|
| *path* (req)  | {base_uri}/search |
| *body*        | JSON of the search request|

**Request**

```ruby
curl \
-H "Authorization: Basic {Authorization Token}" \
-H "Content-Type: application/json" \
-X POST --data "@search.json" 'https://bcstage.api.loop.bluecats.com/search'
```

## Search Body   

The search topics send up a body of JSON. The first required key:value pair is *from:resource*. One of the 3 following string types of resource must be picked from as a resource:

- users
- objects
- groups

If you were to query the *users* resource, you would format it as:

```
{
    'from': 'users',
    
```
If you were to query the *groups* resource, you would format it as:

```
{
    'from': 'groups',
    
```

If you were to query the *object* resource, then a valid objectType of type is string is required in the base level of the JSON. For the rest of our example, we are going to be querying for a car object. 


```
{
    'objectType': 'car', 
    
```

The next required key is *select*, which is of the list type. Within the select list, can either select for { 'value': 'type' } and/or { 'count': 'all' }. Count returns the total number of the object queried. We can query things the year, color, and count all car objectTypes that you have ownership of. This example we will select by year, color, and count all of the types. 

```
{
    {
    'objectType': 'car',
    'select': [
        { 'value': 'year' },
        { 'value': 'color' },
        { 'count': 'all' }
    ],
```
	
The next few parameters are not required: 

- *offset* (integer): defaults to 0 and is where to start indexing the objects from
- *limit* (integer): defaults to 100 and can be between the values 0-1000
- *orderBy* (list of dictionaries): requires at at least one dictionary and has the form of key:value with direction being either ascending or descending: {'value': {key}, 'direction': asc/desc}
- *ssearch* (string): this allows you to search any string

To add to our car example, we will:

- offset the index by 5
- limit the results to 50 
- orderBy:
	1. the year by descending 
	2. color by ascending
- ssearch by 'cool car' 

```
{
    'objectType': 'car',
    'select': [
        { 'value': 'year' },
        { 'value': 'color' },
        { 'count': 'all' }
    ],
    'offset': 5,
    'limit': 50,
    'orderBy': [
        { 'value': 'year', 'direction': 'desc' },
        { 'value': 'color', 'direction', 'asc' }
    ],
    'ssearch': 'cool car',
```

## Search Filters: *where*

*where* is a list that allows you to filter on your searches with a few powerful concepts that need to be explained: clause dictionaries (composed of comparison operations), query clause, and clause lists.
The bare minimum where filter is using a *clause dictionary*. 

### Clause Dictionary

This is a form of a dictionary that allows  *comparison operations* to be performed on keys with some type of value. This takes the form of: 

```
{'key_name': {'comparison operation': 'value' }}
```
This is all the possible current comparison operations available. 

| Comparison Operation Name| Explanation | Example |
|--------------------------|-------------|---------|
| EQ | filters if the value is equal |{'year': {'EQ': 2000 }}|
| NEQ | filters if the value is not equal |{'year': {'NEQ': 400 }}|
| GT | filters if the value is greater than |{'age': {'GT': 25 }}|
| GE | filters if the value is greater than or equal to |{'age': {'GE': 25 }}|
| LT | filters if the value is less than |{'age': {'LT': 25 }}|
| LE | filters if the value is less than or equal to |{'age': {'LE': 25 }}|
| HAS | filters if the key has the value (Requires the string 'value') |{'age': {'HAS': 'value' }}|
| HASNO | filters if the key has the value (Requires the string 'value') |{'age': {'HASNO': 'value' }}|


### Comparison Operation Available for Types 

| Types | Comparison Operations Types Available  | 
|---------------|-----------------|
| *string*      | ALL |
| *dateTime*    | ALL |
| *decimal*     | ALL |
| *int*         | ALL |
| *bool*        | EQ, NEQ, HAS, HASNO |
| *json*        | HAS, HASNO |

### Query List  

A query clause is the use of AND or OR. A query list allows is a list type that requires a query clause first and then allows clause dictionaries and/or query lists to be nested. Here is an example of a basic query list with a two clause dictionaries. 

```
where [
    {clause dictionary 1}, 
    {clause dictionary 2}
]
```

Here is an example of a basic query list with a two clause dictionaries and a nested query list. This queries for when (clause 1 AND clause 2) are true. 


```
where [
    {
        "or":
            [
                {clause dictionary 1}, 
                {
                    "and": {
                        {clause dictionary 2}, 
                        {clause dictionary 3}
                    }
                }
            ]
    }
]
```

Here is an example of a basic query list with a two clause dictionaries and a nested query list. This queries for when (clause 1 OR (clause 2 AND clause 3)) are true. 

This is a where filter with filtering a car by the colors: (green) OR (red) OR (blue AND white). The final JSON body for our car example would like: 

```
{
    'from': 'car',
    'select': [
        { 'value': 'year' },
        { 'value': 'color' },
        { 'count': 'all' }
    ],
    'offset': 5,
    'limit': 50,
    'orderBy': [
        { 'value': 'year', 'direction': 'desc' },
        { 'value': 'color', 'direction', 'asc' }
    ],
    'ssearch': 'cool car',
    'where': [
     	{'or': [
        		{ 'color': { 'EQ': 'red' } },
        		{ 'color': { 'EQ': 'green'} }, 
        		{'and': [
		       			{ 'color': { 'EQ': 'blue' } },
		       			{ 'color': { 'EQ': 'white' } }
		       		]
		      	}
        	]
     	}	         
    ]
}
```



**Example File: search.json** 

```
{
  "from": "bcdEquipment",
  "select": [ 
    {"count": "all" },
    {"value": "serialNumber"}
    ],
  "where":[
    {"place" : {"HASNO": "value" }}
  ]
}
```

**Request** 

```ruby 
curl \ 
-H "Authorization: Basic ZXJuaWVAYmx1ZWNhdHMuY29tOnBhc3N3b3Jk" \
-H "Content-Type: application/json" \
-X POST --data "@search.json" 'https://bcstage.api.loop.bluecats.com/search' >> bcdEquipment.json
```
	
	