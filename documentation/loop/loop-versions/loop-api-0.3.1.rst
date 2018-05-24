This describes the resources for the Loop API. If you have any problems
or requests contact support@bluecats.com.

New Features:
-------------

-  For Event Queries, ObjectID is now required and limit is now
   optional.
-  Object create and edit response has changed to return json dictionary
   with objectType and id e.g. {“objectType”: “Device”, “id”:
   “4b3890e4fb580c6e9443b6081d0a1628”}.
-  Search: from can include log type.
-  Add header X-API-HEADER to EVERY request, value of 1.
-  Groups now have placeParentID in schema.

Table of Contents:
------------------

-  `Introduction <loop-api-0.3.1#introduction>`__

   -  `Connecting to the API <loop-api-0.3.1#connecting-to-the-api>`__

-  `API Endpoints <loop-api-0.3.1#api-endpoints>`__

   -  `Login <loop-api-0.3.1#login>`__
   -  `Schema Query <loop-api-0.3.1#schema-query>`__

      -  `Optional Query
         Params <loop-api-0.3.1#optional-query-params>`__
      -  `Data Types <loop-api-0.3.1#data-types>`__

   -  `Create Objects <loop-api-0.3.1#create-objects>`__
   -  `Edit Objects <loop-api-0.3.1#edit-objects>`__
   -  `Delete Objects <loop-api-0.3.1#delete-objects>`__
   -  `Event Query Params <loop-api-0.3.1#event-query-params>`__

      -  `Event Identifiers
         Examples <loop-api-0.3.1#event-identifiers-examples>`__

   -  `Posting Events <loop-api-0.3.1#posting-events>`__

      -  `Event Body <loop-api-0.3.1#event-body>`__

   -  `Search Endpoint <loop-api-0.3.1#search-endpoint>`__
   -  `Post Search <loop-api-0.3.1#post-search>`__
   -  `Search Body <loop-api-0.3.1#search-body>`__
   -  `Search Filters <loop-api-0.3.1#search-filters-where>`__

      -  `Clause Dictionary <loop-api-0.3.1#clause-dictionary>`__
      -  `Comparison Operation Available for
         Types <loop-api-0.3.1#comparison-operation-available-for-types>`__
      -  `Query List <loop-api-0.3.1#query-list>`__

Introduction
------------

The Loop API allows you to:

-  Create or update a single or multiple (batch) Groups or Objects
-  Change the password of the Group
-  Delete an Object
-  Claim or unclaim a single or multiple (batch) Objects by the Owner
-  Send batch Events
-  Search and perform analytics on Groups, Objects, and Events

Connecting to the API
---------------------

+-----------------------+-----------------------+-----------------------+
| Environment           | Connectivity          | PORT                  |
+=======================+=======================+=======================+
| *Labs*                | URL:                  | 443 (default https    |
|                       | https://bcstage.api.l | port)                 |
|                       | oop.bluecats.com/     |                       |
+-----------------------+-----------------------+-----------------------+
| *Production*          | URL: {base_uri}       | 443 (default https    |
|                       |                       | port)                 |
+-----------------------+-----------------------+-----------------------+

API Endpoints
=============

This section contains outlines for API Endpoints and JSON structures
with examples using `cURL <https://github.com/curl/curl>`__.

Login
-----

Login credentials are returned after a POST request with your email and
password in the body as JSON for user login. *Basic Authorization
Credentials* is returned after a POST request for user login. Adding the
*Basic Authorization Credentials* to your header allows you to make
requests POST/GET groups.

+---------------+-------------------------------------------------+
| HTTPS Element | Request Details                                 |
+===============+=================================================+
| *method*      | POST                                            |
+---------------+-------------------------------------------------+
| *header*      | Content-Type: application/json, X-API-HEADER: 1 |
+---------------+-------------------------------------------------+
| *path*        | {base_uri}/login                                |
+---------------+-------------------------------------------------+
| *body*        | { “email”: {email}, “password”: {password} }    |
+---------------+-------------------------------------------------+

**Request**

.. code:: ruby

   curl \
   -H "Content-Type: application/json" \
   -H "X-API-HEADER: 1" \
   -X POST --data "@login.json" 'https://bcstage.api.loop.bluecats.com/login'

**Response**

.. code:: ruby

   {
       "auth": "Basic {Authorization Token}",
       "groupID": "764166c0c49c4e113c62f0ca3cecd84e", 
       "email": "{email}",
       "firstName": "{firstName}", 
       "lastName": "{lastName}"
   }

Schema Query
------------

*Schema* replaces the object-types endpoint. Performing a GET request on
*Schema* is useful to determine which objects are available to which
groups and which you can query. The group ID in the objectTypes shows
what group created and has ownership of the objectType. Anyone
underneath that group can create objects. When creating objects, check
which object keys are required and what types they are when you want to
create objects.

+--------------------------------+-------------------------------------+
| HTTPS Element                  | Request Details                     |
+================================+=====================================+
| *method*                       | GET                                 |
+--------------------------------+-------------------------------------+
| *header*                       | Authorization: Basic {credentials}, |
|                                | Content-Type: application/json,     |
|                                | X-API-HEADER: 1                     |
+--------------------------------+-------------------------------------+
| *path*                         | {base_uri}/schema                   |
+--------------------------------+-------------------------------------+

Schema Query Params
~~~~~~~~~~~~~~~~~~~

+-----------------+------------+-----------+---------------+----------+
| **Key**         | **Value**  | Required  | Explanation   | Example  |
+=================+============+===========+===============+==========+
| incUIAttr       | *bool*     | No        | Exposes       | incUIAtt |
|                 |            |           | hidden UI     | r=True   |
|                 |            |           | Attributes    |          |
|                 |            |           | within        |          |
|                 |            |           | objectTypes   |          |
+-----------------+------------+-----------+---------------+----------+
| groupID         | *string*   | No        | String        | groupID= |
|                 |            |           | filters down  | {groupID |
|                 |            |           | objectTypes   | }        |
|                 |            |           | based on      |          |
|                 |            |           | group access  |          |
+-----------------+------------+-----------+---------------+----------+

Data Types
~~~~~~~~~~

When you query the *Schema* endpoint, you can display types and key
information for the resources for users, groups, objects, objectTypes.
Everything stored in the database are these six data types.

+------------------------+----------------------------+----------------+
| Type                   | Description                | Example        |
+========================+============================+================+
| *string*               | length of string field     | “firstName”    |
|                        | that defaults to size 100  |                |
+------------------------+----------------------------+----------------+
| *datetime*             | ISO combined date in time  | 2017-10-31T18: |
|                        | for UTC format             | 44:38.129120Z  |
+------------------------+----------------------------+----------------+
| *decimal*              | 9 digits on each side of   | 34.2338        |
|                        | the decimal point, if      |                |
|                        | bigger than it will be     |                |
|                        | truncated                  |                |
+------------------------+----------------------------+----------------+
| *int*                  | 32-bits                    | 56             |
+------------------------+----------------------------+----------------+
| *json*                 | application/json format    | {“email”:{emai |
|                        |                            | l},            |
|                        |                            | “password”:{pa |
|                        |                            | ssword}}       |
+------------------------+----------------------------+----------------+
| *bool*                 | Only accepts True or False | True           |
+------------------------+----------------------------+----------------+

For each object type, it has a list of keys. Each key has a type, field,
and attributes. The keys have attributes like being if it is required,
if the type is editable, or if it has a linked to another object.

.. code:: ruby


   {
       "name": "groupID",
       "displayName": "Group ID",
       "required": true,
       "type": "string",
       "length": 32,
       "editable": true,
       "link": "groups"
   }

In our example below, we will query the *Schema* endpoint. There are
ellipses to shorten the very long schema response.

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -H "X-API-HEADER: 1" \
   -X GET 'https://bcstage.api.loop.bluecats.com/schema?incUIAttr=True'

**Response**

.. code:: ruby


   {
       "users": {
           "keys": [
               {
                   "name": "email",
                   "displayName": "Email",
                   "type": "string",
                   "length": 100
               },
               {
                   "name": "firstName",
                   "displayName": "First Name",
                   "type": "string",
                   "length": 100
               },
               {
                   "name": "lastName",
                   "displayName": "Last Name",
                   "type": "string",
                   "length": 100
               },
               {
                   "name": "groupID",
                   "displayName": "Group",
                   "type": "string",
                   "length": 100,
                   "link": "groups"
               }
           ]
       },
       "groups": {
           "keys": [
               {
                   "name": "id",
                   "displayName": "ID",
                   "type": "string",
                   "length": 32
               },
               {
                   "name": "type",
                   "displayName": "Type",
                   "type": "string",
                   "length": 100
               },
               {
                   "name": "parentID",
                   "displayName": "Parent Group",
                   "type": "string",
                   "length": 32,
                   "link": "groups"
               },
             
               
               . . .

           ]
       },
       "objectTypes": [
           "bcdEquipment",
           "bcdParcel",
           "bcdPerson",
           "bcdTableFlag",
           "bcdVehicle",
           "Device",
           "Place"
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
            
                    . . .
                   {
                       "name": "serialNumber",
                       "displayName": "Serial Number",
                       "required": false,
                       "type": "string",
                       "editable": true,
                       "length": 100
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
                  
               ],
               "groupID": "a3cec63e2aa927c93567abd968677314"
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
                   
                   . . . 
                   
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
                   }
               ],
               "groupID": "a3cec63e2aa927c93567abd968677314"
           },

           
              . . .

       "logTypes": [
           "x_placelog",
           "x_triplog"
       ],
       "logs": {
           "x_placelog": {
               "name": "x_placelog",
               "displayName": "Place Logs",
               "groupID": "764166c0c49c4e113c62f0ca3cecd84e",
               "keys": [
                   {
                       "name": "ts",
                       "displayName": "Timestamp",
                       "type": "datetime"
                   },
                   {
                       "name": "objectid",
                       "displayName": "Object ID",
                       "type": "string",
                       "length": 32
                   },
                   {
                       "name": "objecttype",
                       "displayName": "Object Type",
                       "type": "string",
                       "length": 100
                   },
                   {
                       "name": "groupid",
                       "displayName": "Group ID",
                       "type": "string",
                       "length": 32
                   }
               ],
               
               "x_triplog": {
               "name": "x_triplog",
               "displayName": "Trip Logs",
               "groupID": "979a3ef98cfb4ba37fd4cae28fea597",
               "keys": [
                   {
                       "name": "ts",
                       "displayName": "Timestamp",
                       "type": "datetime"
                   },
                   {
                       "name": "objectid",
                       "displayName": "Object ID",
                       "type": "string",
                       "length": 32
                   },

                   . . .
                   

        

Create Objects
--------------

In order to Create Objects, you will POST to the *Objects* endpoint. To
create objects, you will POST body JSON with:

-  objectType in body
-  groupID
-  required keys
-  and all of the optional keys.

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | POST                                           |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects                             |
+---------------------+------------------------------------------------+
| *body*              | { “groupID”: {groupID}, “objectType”:          |
|                     | {objectType}, “requiredKey”: {requiredValue},  |
|                     | “optionalKey”: {optionalValue} }               |
+---------------------+------------------------------------------------+

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -X POST --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'

Object create and edit response returns a json dictionary with
objectType and id.

**Response**

.. code:: ruby

   "objectType": "Device", 
   "id": "4b3890e4fb580c6e9443b6081d0a1628"

Edit Objects
------------

In order to Edit Objects, you will PATCH to the *Objects* endpoint. To
edit objects, you will PATCH body JSON with:

-  objectID and object type in the body
-  any keys you want to change
-  You can change -> groupID if it is in your groups

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | PATCH                                          |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects                             |
+---------------------+------------------------------------------------+
| *body*              | { “id”: {id}, “objectType”: {objectType},      |
|                     | “editableKey”: {editableValue} }               |
+---------------------+------------------------------------------------+

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -X PATCH --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'

Object create and edit response returns a json dictionary with
objectType and id.

**Response**

.. code:: ruby

   "objectType": "Device", 
   "id": "4b3890e4fb580c6e9443b6081d0a1628"

Delete Objects
--------------

In order to delete Objects, you will DELETE to the *Objects* endpoint.
To edit objects, you will DELETE body JSON with:

-  objectType
-  ID

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | DELETE                                         |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects                             |
+---------------------+------------------------------------------------+
| *body*              | { “id”: {id}, “objectType”: {objectType}}      |
+---------------------+------------------------------------------------+

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -X DELETE --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'

**Response**

.. code:: ruby

   "success"

.. raw:: html

   <!---
   /dman/snLookup POST 
   check if BC beacon is registered with the API and if its already linked to another object 
   if it does than it has 
   WAIT
   --->

Query Events
------------

Login credentials are returned after a GET request for user login. Any
event type located here. Those event types will be saved. However, it
will not necessarily have state logic applied to it.

+--------------------------------+-------------------------------------+
| HTTPS Element                  | Request Details                     |
+================================+=====================================+
| *method*                       | GET                                 |
+--------------------------------+-------------------------------------+
| *header*                       | Authorization: Basic {credentials}, |
|                                | Content-Type: application/json,     |
|                                | X-API-HEADER: 1                     |
+--------------------------------+-------------------------------------+
| *path*                         | {base_uri}/events                   |
+--------------------------------+-------------------------------------+

Event Query Params
------------------

+-------------+--------------+-------------+-------------+-----------+
| **Key**     | **Value**    | Required    | Explanation | Examples  |
+=============+==============+=============+=============+===========+
| objectType  | *string*     | Yes         | specifying  | objectTyp |
|             |              |             | identifier  | e={object |
|             |              |             | to query    | Type}     |
+-------------+--------------+-------------+-------------+-----------+
| objectID    | *string*     | Yes         | specifying  | objectID= |
|             |              |             | identifier  | {objectID |
|             |              |             | to query    | }         |
+-------------+--------------+-------------+-------------+-----------+
| eventType   | *string*     | No          | [event      | relative_ |
|             |              |             | types]({{   | url       |
|             |              |             | ‘documentat | }})       |
|             |              |             | ion/loop/bc |           |
|             |              |             | -loop-event |           |
|             |              |             | -types’     |           |
+-------------+--------------+-------------+-------------+-----------+
| lastKeyID   | *string*     | No          | Starts from | “lastKeyI |
|             |              |             | the event   | D”:       |
|             |              |             | after       | “bcId#500 |
|             |              |             | referenced. | 02D7D”,   |
|             |              |             | (*Requires* | “lastKeyT |
|             |              |             | lastKeyTS)  | S”:       |
|             |              |             |             | “2017-11- |
|             |              |             |             | 01T17:34: |
|             |              |             |             | 59.801528 |
|             |              |             |             | Z”        |
+-------------+--------------+-------------+-------------+-----------+
| lastKeyTS   | *string*     | No          | Starts from | “lastKeyI |
|             |              |             | the event   | D”:       |
|             |              |             | after       | “bcId#500 |
|             |              |             | referenced. | 02D7D”,   |
|             |              |             | (*Requires* | “lastKeyT |
|             |              |             | lastKeyID)  | S”:       |
|             |              |             |             | “2017-11- |
|             |              |             |             | 01T17:34: |
|             |              |             |             | 59.801528 |
|             |              |             |             | Z”        |
+-------------+--------------+-------------+-------------+-----------+
| limit       | *int*        | No          | Number of   | count=34  |
|             |              |             | objects     |           |
|             |              |             | returned    |           |
|             |              |             | (max 100)   |           |
+-------------+--------------+-------------+-------------+-----------+

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -X GET "https://bcstage.api.loop.bluecats.com/events?"\
       "objectType=bcdPerson&"\
       "objectID=b5d3daebcad1bef1ff660d13ec80a1df"

**Response**

.. code:: ruby


   {
       "events": [
           {
               "ts": "2018-01-17T00:00:00.000031Z",
               "eventID": "eddyUID#3EBC2C6462A6D9E7BF47000000000003",
               "objectType": "bcdPerson",
               "objectID": "b5d3daebcad1bef1ff660d13ec80a1df",
               "edgeMAC": "E4956E4CC2E0",
               "objectType_id_eventType": "bcdPerson_b5d3daebcad1bef1ff660d13ec80a1df_scanEvent",
               "groupID": "66679cbce97e147f3ef61122eb63a1a3",
               "objectType_id": "bcdPerson_b5d3daebcad1bef1ff660d13ec80a1df",
               "eventType": "scanEvent",
               "eventBody": {
                   "mac": "D42202000053",
                   "temp": "test_temp",
                   "eddyUID": "3EBC2C6462A6D9E7BF47000000000003"
               }
           }
       ],
       "count": 1,
       "lastKeyID": "bcdPerson_b5d3daebcad1bef1ff660d13ec80a1df",
       "lastKeyTS": "2018-01-17T00:00:00.000031Z"
   }

Posting Events
==============

+---------------+-------------------------------------------------+
| HTTPS Element | Request Details                                 |
+===============+=================================================+
| *method*      | POST                                            |
+---------------+-------------------------------------------------+
| *header*      | Content-Type: application/json, X-API-HEADER: 1 |
+---------------+-------------------------------------------------+
| *path*        | {base_uri}/events                               |
+---------------+-------------------------------------------------+
| *body*        | {event body}                                    |
+---------------+-------------------------------------------------+

Event Body
~~~~~~~~~~

In general for the Event Body, you will have a edgeMac identifier this
is where the Events are being posted from.

::

   {
       "edgeMac":{identifier} 

If you have GPS values, you can have a location dictionary with key
value pairs for lat and long. Including values for Lat and long will
create a an Edge Location Event. For the locations lat and long, the
value can be empty "" or contain a {value}.

::

       "loc": {  
         "lat": "" or {value},
         "long": "" or {value},
      }

Nested underneath will be an events list. *Time Stamp*, an *identifier*,
and *eventType* is required in each event.

1. Time stamp is required in this UTC ISO-format: DATE:TIME
   (2017-07-20T18:42:34.129120Z).

2. Identifiers are required and can be delivered in two formats:

   -  The edge relay format for:

      -  iBeac
      -  eddyUID
      -  bcId
      -  btAddr

   -  Identifier key value pair:

      -  identifier=ibeac#{ibeaconkeyinhex}
      -  identifier=eddyUID#{eddystoneUID}
      -  identifier=bcId#{bc custom identifier, normally serial number}
      -  identifier=btAddr#{bluetooth address}
      -  identifier=Whatever {though this does not mean it will be
         processed by State Logic}

3. The final thing required for this eventType. There are many different
   [event types]({{ ‘/documentation/loop/bc-loop-event-types’\|
   relative_url }}). There are other things that can be included in the
   events. It will be saved but does not necessarily mean it will be
   processed.

::

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

Here is a complete example that can be sent up as POST. There are many
different iterations of how you can POST Events. Save an event.json file
to POST.

**Example (event.json)**

::


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

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -H "X-API-HEADER: 1" \
   -X POST --data "@event.json" 'https://bcstage.api.loop.bluecats.com/events'
       

**Request**

.. code:: ruby


   "Success: 3 events reported"
       

Search Endpoint
===============

The search API for Loop can perform queries and analytics on users,
objects, and groups. Our API is based off the `REST
API <https://en.wikipedia.org/wiki/Representational_state_transfer>`__.

The following examples outlines for API Endpoints and JSON structures
with examples using `cURL <https://github.com/curl/curl>`__.

POST search
-----------

A Search API request contains the following elements:

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | POST                                           |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/search                              |
+---------------------+------------------------------------------------+
| *body*              | JSON of the search request                     |
+---------------------+------------------------------------------------+

**Request**

.. code:: ruby

   curl \
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -H "X-API-HEADER: 1" \
   -X POST --data "@search.json" 'https://bcstage.api.loop.bluecats.com/search'

Search Body
-----------

The search topics send up a body of JSON. The first required key:value
pair is *from:resource*. One of the 3 following string types of resource
must be picked from as a resource:

-  users
-  groups
-  object types
-  log types

If you were to query the *users* resource, you would format it as:

::

   {
       'from': 'users',
       

If you were to query the *groups* resource, you would format it as:

::

   {
       'from': 'groups',
       

If you were to query the *object* resource, then a valid objectType of
type is string is required in the base level of the JSON. For the rest
of our example, we are going to be querying for a car object.

::

   {
       'objectType': 'car', 
       

The next required key is *select*, which is of the list type. Within the
select list, can either select for { ‘value’: ‘type’ } and/or { ‘count’:
‘all’ }. Count returns the total number of the object queried. We can
query things the year, color, and count all car objectTypes that you
have ownership of. This example we will select by year, color, and count
all of the types.

::

   {
       {
       'objectType': 'car',
       'select': [
           { 'value': 'year' },
           { 'value': 'color' },
           { 'count': 'all' }
       ],

The next few parameters are not required:

-  *offset* (integer): defaults to 0 and is where to start indexing the
   objects from
-  *limit* (integer): defaults to 100 and can be between the values
   0-1000
-  *orderBy* (list of dictionaries): requires at at least one dictionary
   and has the form of key:value with direction being either ascending
   or descending: {‘value’: {key}, ‘direction’: asc/desc}
-  *ssearch* (string): this allows you to search any string

To add to our car example, we will:

-  offset the index by 5
-  limit the results to 50
-  orderBy:

   1. the year by descending
   2. color by ascending

-  ssearch by ‘cool car’

::

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

Place Parent ID
~~~~~~~~~~~~~~~

Normally, a user can only see information about an resource
(i.e. object, place, etc) if they have ownership of it. The hierarchy
prevents users from seeing resources that are above them from a parent
group. Place Parent ID is a special key that allows a user to see the
place ID of a parent group ID when they normally would not.

::

     "groups": {
           "keys": [
               {
                   "name": "id",
                   "displayName": "ID",
                   "type": "string",
                   "length": 32
               },
               {
                   "name": "parentID",
                   "displayName": "Parent Group",
                   "type": "string",
                   "length": 32,
                   "link": "groups"
               },
             
               
               . . .


               {
                   "name": "placeParentID",
                   "displayName": "Place Access Group",
                   "type": "string",
                   "length": 32,
                   "link": "groups"
               }

Search Filters: *where*
-----------------------

*where* is a list that allows you to filter on your searches with a few
powerful concepts that need to be explained: clause dictionaries
(composed of comparison operations), query clause, and clause lists. The
bare minimum where filter is using a *clause dictionary*.

Clause Dictionary
~~~~~~~~~~~~~~~~~

This is a form of a dictionary that allows *comparison operations* to be
performed on keys with some type of value. This takes the form of:

::

   {'key_name': {'comparison operation': 'value' }}

This is all the possible current comparison operations available.

+--------------------------------------+------------------+------------+
| Comparison Operation Name            | Explanation      | Example    |
+======================================+==================+============+
| EQ                                   | filters if the   | {‘year’:   |
|                                      | value is equal   | {‘EQ’:     |
|                                      |                  | 2000 }}    |
+--------------------------------------+------------------+------------+
| NEQ                                  | filters if the   | {‘year’:   |
|                                      | value is not     | {‘NEQ’:    |
|                                      | equal            | 400 }}     |
+--------------------------------------+------------------+------------+
| GT                                   | filters if the   | {‘age’:    |
|                                      | value is greater | {‘GT’: 25  |
|                                      | than             | }}         |
+--------------------------------------+------------------+------------+
| GE                                   | filters if the   | {‘age’:    |
|                                      | value is greater | {‘GE’: 25  |
|                                      | than or equal to | }}         |
+--------------------------------------+------------------+------------+
| LT                                   | filters if the   | {‘age’:    |
|                                      | value is less    | {‘LT’: 25  |
|                                      | than             | }}         |
+--------------------------------------+------------------+------------+
| LE                                   | filters if the   | {‘age’:    |
|                                      | value is less    | {‘LE’: 25  |
|                                      | than or equal to | }}         |
+--------------------------------------+------------------+------------+
| HAS                                  | filters if the   | {‘age’:    |
|                                      | key has the      | {‘HAS’:    |
|                                      | value (Requires  | ‘value’ }} |
|                                      | the string       |            |
|                                      | ‘value’)         |            |
+--------------------------------------+------------------+------------+
| HASNO                                | filters if the   | {‘age’:    |
|                                      | key has the      | {‘HASNO’:  |
|                                      | value (Requires  | ‘value’ }} |
|                                      | the string       |            |
|                                      | ‘value’)         |            |
+--------------------------------------+------------------+------------+

Comparison Operation Available for Types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------+---------------------------------------+
| Types      | Comparison Operations Types Available |
+============+=======================================+
| *string*   | ALL                                   |
+------------+---------------------------------------+
| *dateTime* | ALL                                   |
+------------+---------------------------------------+
| *decimal*  | ALL                                   |
+------------+---------------------------------------+
| *int*      | ALL                                   |
+------------+---------------------------------------+
| *bool*     | EQ, NEQ, HAS, HASNO                   |
+------------+---------------------------------------+
| *json*     | HAS, HASNO                            |
+------------+---------------------------------------+

Query List
~~~~~~~~~~

A query clause is the use of AND or OR. A query list allows is a list
type that requires a query clause first and then allows clause
dictionaries and/or query lists to be nested. Here is an example of a
basic query list with a two clause dictionaries.

::

   where [
       {clause dictionary 1}, 
       {clause dictionary 2}
   ]

Here is an example of a basic query list with a two clause dictionaries
and a nested query list. This queries for when (clause 1 AND clause 2)
are true.

::

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

Here is an example of a basic query list with a two clause dictionaries
and a nested query list. This queries for when (clause 1 OR (clause 2
AND clause 3)) are true.

This is a where filter with filtering a car by the colors: (green) OR
(red) OR (blue AND white). The final JSON body for our car example would
like:

::

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

For example, let’s say that you want to replace low batteries for your
equipment (i.e. bcdEquipment). You would want know the equipments:

-  battery level
-  serial number
-  where that equipment was last seen

You would search from bcdEquipment, select for SN battery life and
place. Then you would check if the equipment has a place AND if it has a
battery less then 50%.

**Example File: search.json**

::

   {
     "from": "bcdEquipment",
     "select": [ 
       {"count": "all" },
       {"value": "serialNumber"},
       {"value": "batterySoC"},
       {"value": "place"}
       
       ],
     "where":[
       {"and": [
                   {"place" : {"HAS": "value" }},
                   {"batterySoC": {"LT": 50}}
               ]
           }
       
     ]
   }

**Request**

.. code:: ruby

   curl \ 
   -H "Authorization: Basic {Authorization Token}" \
   -H "Content-Type: application/json" \
   -H "X-API-HEADER: 1" \
   -X POST --data "@search.json" 'https://bcstage.api.loop.bluecats.com/search' >> bcdEquipment.json

**Respone**

.. code:: ruby


   {
       "count": 2,
       "bcdEquipment": [
           {
               "place": "ab18fb87bddf4f23a31b6bc9f9b29844",
               "batterySoC": 47,
               "serialNumber": "50000193"
           },
           {
               "place": "ab18fb87bddf4f23a31b6bc9f9b29844",
               "batterySoC": 49,
               "serialNumber": "5000083D"
           }
       ]
   }

