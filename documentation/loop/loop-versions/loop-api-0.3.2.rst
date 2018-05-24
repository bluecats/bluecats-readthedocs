This describes the resources for the Loop API. If you have any problems
or requests contact support@bluecats.com.

New Features:
-------------

-  Schema field descriptions - Basically a list of all the keys that
   could be in a schema object and what they mean.

-  Search now supports IN for where clauses. Example Below:

::

       {
         "where": [
           {
             "id": {
               "IN": [
                 "0a6afd273021c150d4d758613a70c6b3",
                 "28e92020a3cc0939bf597a97dce99cce"
               ]
             }
           }
         ]
       }

Table of Contents:
------------------

-  `Introduction <loop-api-0.3.2#introduction>`__
-  `Connecting to the API <loop-api-0.3.2#connecting-to-the-api>`__
-  **Starting with Loop API**

   -  `Login <loop-api-0.3.2#login>`__
   -  `Schema Query <loop-api-0.3.2#schema-query>`__

      -  `Optional Query
         Params <loop-api-0.3.2#optional-query-params>`__
      -  `Data Types <loop-api-0.3.2#data-types>`__
      -  `Schema Field
         Descriptions <loop-api-0.3.2#schema-field-descriptions>`__

         -  `Key Field
            Descriptions <loop-api-0.3.2#key-field-descriptions>`__
         -  `UI Field
            Descriptions <loop-api-0.3.2#ui-field-descriptions>`__

-  **Objects**

   -  `Create Objects <loop-api-0.3.2#create-objects>`__
   -  `Edit Objects <loop-api-0.3.2#edit-objects>`__
   -  `Delete Objects <loop-api-0.3.2#delete-objects>`__
   -  **Object Links**

      -  `Create Object Links <loop-api-0.3.2#create-object-links>`__
      -  `Edit Object Links <loop-api-0.3.2#edit-object-links>`__
      -  `Delete Object Links <loop-api-0.3.2#delete-object-links>`__

-  **Events**

   -  `Event Query Params <loop-api-0.3.2#event-query-params>`__

      -  `Event Identifiers
         Examples <loop-api-0.3.2#event-identifiers-examples>`__

   -  `Posting Events <loop-api-0.3.2#posting-events>`__

      -  `Event Body <loop-api-0.3.2#event-body>`__

-  **Search**

   -  `Search Endpoint <loop-api-0.3.2#search-endpoint>`__
   -  `Post Search <loop-api-0.3.2#post-search>`__
   -  `Search Body <loop-api-0.3.2#search-body>`__
   -  `Search Filters <loop-api-0.3.2#search-filters-where>`__

      -  `Clause Dictionary <loop-api-0.3.2#clause-dictionary>`__
      -  `Comparison Operation Available for
         Types <loop-api-0.3.2#comparison-operation-available-for-types>`__
      -  `Query List <loop-api-0.3.2#query-list>`__

Introduction
------------

The API is the interface between the customers and Loop. Our API is
based off of `RESTful
API <https://en.wikipedia.org/wiki/Representational_state_transfer>`__
calls.

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

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X POST --data "@login.json" '{base_url}/login'
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       {
           "auth": "Basic {Authorization Token}",
           "groupID": {groupID}, 
           "email": "{email}",
           "firstName": "{firstName}", 
           "lastName": "{lastName}"
       }
       </code></pre>

.. raw:: html

   </details>

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

Schema Field Descriptions
~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------+--------------------+--------------------+
| Schema Field Name          | Type               | Description        |
+============================+====================+====================+
| name                       | *string*           | unique name of     |
|                            |                    | object type        |
+----------------------------+--------------------+--------------------+
| displayName                | *string*           | non-unique name    |
|                            |                    | for use in         |
|                            |                    | displays           |
+----------------------------+--------------------+--------------------+
| class                      | *Thing* or *Place* | class of object    |
|                            |                    | type               |
+----------------------------+--------------------+--------------------+
| groupID                    | *string*           | the id of the      |
|                            |                    | group this object  |
|                            |                    | type is defined    |
|                            |                    | for                |
+----------------------------+--------------------+--------------------+
| keys                       | *list*             | list of object     |
|                            |                    | keys and their     |
|                            |                    | parameters *see    |
|                            |                    | key table below*   |
+----------------------------+--------------------+--------------------+
| uiAttr                     | *dictionary*       | Dictionary of      |
|                            |                    | object keys and    |
|                            |                    | their ui           |
|                            |                    | parameters order   |
|                            |                    | of keys in the     |
|                            |                    | dictionary         |
|                            |                    | represents the     |
|                            |                    | order keys should  |
|                            |                    | be displayed in a  |
|                            |                    | list view *see     |
|                            |                    | uiAttr Key table   |
|                            |                    | below*             |
+----------------------------+--------------------+--------------------+

Key Field Descriptions
~~~~~~~~~~~~~~~~~~~~~~

+----------------------------+--------------------+--------------------+
| Key Field Name             | Type               | Description        |
+============================+====================+====================+
| name                       | *string*           | Unique name of the |
|                            |                    | key                |
+----------------------------+--------------------+--------------------+
| displayName                | *string*           | Non-unique name    |
|                            |                    | for use in         |
|                            |                    | displays           |
+----------------------------+--------------------+--------------------+
| required                   | *bool*             | Whether the key    |
|                            |                    | value is required  |
|                            |                    | on creation or not |
+----------------------------+--------------------+--------------------+
| type                       | *string*, *json*,  | Type of the key’s  |
|                            | *datetime*, *int*, | value              |
|                            | *decimal*, or      |                    |
|                            | *bool*             |                    |
+----------------------------+--------------------+--------------------+
| length                     | *int*              | Length of value    |
|                            |                    | (if type is        |
|                            |                    | string)            |
+----------------------------+--------------------+--------------------+
| editable                   | *bool*             | Whether the key    |
|                            |                    | value is able to   |
|                            |                    | be edited          |
+----------------------------+--------------------+--------------------+
| link                       | *string*           | If present, this   |
|                            |                    | represents that    |
|                            |                    | the key value is a |
|                            |                    | reference to       |
|                            |                    | another object.    |
|                            |                    | The value of link  |
|                            |                    | is the object type |
|                            |                    | name that this key |
|                            |                    | is a reference to  |
+----------------------------+--------------------+--------------------+

UI Field Descriptions
~~~~~~~~~~~~~~~~~~~~~

+----------------------------+--------------------+--------------------+
| uiAttr Key Field Name      | Type               | Description        |
+============================+====================+====================+
| hidden                     | *bool*             | whether the key    |
|                            |                    | should be hidden   |
|                            |                    | or not in the list |
|                            |                    | view               |
+----------------------------+--------------------+--------------------+
| input                      | *string*           | The input type     |
|                            |                    | that should be     |
|                            |                    | used for this key  |
|                            |                    | in forms. If input |
|                            |                    | is not present,    |
|                            |                    | then value is      |
|                            |                    | assumed to be      |
|                            |                    | ‘textbox’. If the  |
|                            |                    | value is ‘special’ |
|                            |                    | then the ui should |
|                            |                    | already have       |
|                            |                    | special code in    |
|                            |                    | place for it.      |
+----------------------------+--------------------+--------------------+
| detailsLink                | *dictionary*       | describes a URL to |
|                            |                    | provide for more   |
|                            |                    | details pertaining |
|                            |                    | to this key        |
|                            |                    | *detailsLink       |
|                            |                    | dictionary example |
|                            |                    | below*             |
+----------------------------+--------------------+--------------------+

**detailsLink dictionary example below:**

.. code:: ruby

   {
       "path": ["/things", "objecttype", "objectid"],
       "queryParams": {
            "showtab": "logs"
       }
   }

In our example below, we will query the *Schema* endpoint. There are
ellipses to shorten the very long schema response.

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X GET '{base_url}/schema?incUIAttr=True'
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       {
           "objectLinks": {
               "keys": [
                   {
                       "name": "eventIDType",
                       "displayName": "Event ID Type",
                       "type": "string",
                       "length": 100
                   },
                   {
                       "name": "eventID",
                       "displayName": "Event ID",
                       "type": "string",
                       "length": 100
                   },
                   {
                       "name": "objectType",
                       "displayName": "Object Type",
                       "type": "string",
                       "length": 100
                   },
                   {
                       "name": "objectID",
                       "displayName": "Object ID",
                       "type": "string",
                       "length": 32
                   },
                   {
                       "name": "groupID",
                       "displayName": "Group ID",
                       "type": "string",
                       "length": 32,
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
                       "name": "name",
                       "displayName": "Name",
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
                   {
                       "name": "dmanTeamID",
                       "displayName": "BlueCats Team ID",
                       "type": "string",
                       "length": 40
                   },
                   {
                       "name": "abbreviation",
                       "displayName": "Abbreviation",
                       "type": "string",
                       "length": 100
                   },
                   
                    . . .
               
                   }
               ]
           },
       
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
                  . . .
       
           "logs": {
                   
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
               "logTypes": [
               "x_battlog",
               "x_intransitlog",
               "x_placelog",
               "x_positionlog",
               "x_templog",
               "x_triplog"
           ]
       }           
       </code></pre>

.. raw:: html

   </details>

Objects
=======

Are what allow you to give what you want to track meaningful data and
link it to Loop.

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

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -X POST --data "@object.json" 'https://bcstage.api.loop.bluecats.com/objects'
       </code></pre>

.. raw:: html

   </details>

Object create and edit response returns a json dictionary with
objectType and id.

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "objectType": "Device", 
       "id": "4b3890e4fb580c6e9443b6081d0a1628"
       </code></pre>

.. raw:: html

   </details>

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

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -X PATCH --data "@object.json" '{base_url}/objects'
       </code></pre>

.. raw:: html

   </details>

Object create and edit response returns a json dictionary with
objectType and id.

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "objectType": "Device", 
       "id": "4b3890e4fb580c6e9443b6081d0a1628"
       </code></pre>

.. raw:: html

   </details>

Delete Objects
--------------

In order to delete Objects, you will DELETE to the *Objects* endpoint.
To edit objects, you will DELETE body JSON with:

-  objectType
-  ID

+-----------------------------------+-----------------------------------+
| HTTPS Element                     | Request Details                   |
+===================================+===================================+
| *method*                          | DELETE                            |
+-----------------------------------+-----------------------------------+
| *header*                          | Authorization: Basic              |
|                                   | {credentials}, Content-Type:      |
|                                   | application/json                  |
+-----------------------------------+-----------------------------------+
| *path* (req)                      | {base_uri}/objects                |
+-----------------------------------+-----------------------------------+
| *body*                            | { “id”: {id}, “objectType”:       |
|                                   | {objectType}}                     |
+-----------------------------------+-----------------------------------+

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -X DELETE --data "@object.json" '{base_url}/objects'
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "objectType": "Device", 
       "id": "4b3890e4fb580c6e9443b6081d0a1628"
       </code></pre>

.. raw:: html

   </details>

Object Links
------------

Object Links are what allow you to report events for a specific object
to the Loop API. Each object can have multiple object links or event
identifiers to report back to Loop. With object links, you can: - create
- edit - delete

Create Object Links
~~~~~~~~~~~~~~~~~~~

Adds new object links, single or multiple, potentially for different
objects and types.

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | POST                                           |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects/link                        |
+---------------------+------------------------------------------------+
| *body*              | [[objectType, objectID, eventIDType, eventID], |
|                     | [objectType2, objectID2, eventIDType2,         |
|                     | eventID2]]                                     |
+---------------------+------------------------------------------------+

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X POST --data "@objectslink.json" '{base_url}/objects/link'
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "Success"
       </code></pre>

.. raw:: html

   </details>

Edit Object Links
~~~~~~~~~~~~~~~~~

Replaces object links on single object with PUT links.

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | PUT                                            |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects/link                        |
+---------------------+------------------------------------------------+
| *body*              | {objectType: val, objectID: val, links:        |
|                     | [[eventIDType, eventID], [eventIDType2,        |
|                     | eventID2]}                                     |
+---------------------+------------------------------------------------+

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X PUT --data "@objectslink.json" '{base_url}/objects/link'
       
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "Success"
       </code></pre>

.. raw:: html

   </details>

Delete Object Links
~~~~~~~~~~~~~~~~~~~

Removes object links, single or multiple, potentially for different
objects and types

+---------------------+------------------------------------------------+
| HTTPS Element       | Request Details                                |
+=====================+================================================+
| *method*            | DELETE                                         |
+---------------------+------------------------------------------------+
| *header*            | Authorization: Basic {credentials},            |
|                     | Content-Type: application/json, X-API-HEADER:  |
|                     | 1                                              |
+---------------------+------------------------------------------------+
| *path* (req)        | {base_uri}/objects/link                        |
+---------------------+------------------------------------------------+
| *body*              | [[objectType, objectID, eventIDType, eventID], |
|                     | [objectType2, objectID2, eventIDType2,         |
|                     | eventID2]]                                     |
+---------------------+------------------------------------------------+

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X DELETE --data "@objectlink.json" '{base_url}/objects/link'
       
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "Success"
       </code></pre>

.. raw:: html

   </details>

Events
======

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

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -X GET "{base_url}/events?"\
           "objectType=bcdPerson&"\
           "objectID=b5d3daebcad1bef1ff660d13ec80a1df"
           
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
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
       </code></pre>

.. raw:: html

   </details>

Posting Events
--------------

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
       "edgeMAC":{identifier} 

If you have GPS values, you can have a location dictionary with key
value pairs for lat and long. Including values for Lat and long will
create a an Edge Location Event. For the locations lat and long, the
value can be empty "" or contain a {value}.

::

     "loc":{  
         "lat":     "" or {value},
         "long":    "" or {value},
      },

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

.. raw:: html

   <details>

 Example (event.json)

.. raw:: html

   <pre><code>
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

       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>

       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X POST --data "@event.json" '{base_url}/events'
       
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
       "Success: 3 events reported"
       </code></pre>

.. raw:: html

   </details>

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

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>
       curl \
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X POST --data "@search.json" '{base_url}/search'
       </code></pre>

.. raw:: html

   </details>

Search Body
-----------

The search topics send up a body of JSON. The first required key:value
pair is *from:resource*. One of the 3 following string types of resource
must be picked from as a resource:

-  users
-  groups
-  object types
-  object links
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
       'from': 'car', 
       

The next required key is *select*, which is of the list type. Within the
select list, can either select for { ‘value’: ‘type’ } and/or { ‘count’:
‘all’} OR {‘count’: ‘groups’ }. Count returns the total number of the
object queried. We can query things the year, color, and count all car
objectTypes that you have ownership of. This example we will select by
year, color, and count all of the types.

*count* (all or groups): can now have the value ‘all’ or ‘groups’. {
‘count’: ‘all’ } returns the total number of the object queried.
{‘count’: ‘groups’} will append a ‘count’ key to the returned objects
that has the count of that grouping.

::

   {
       'from': 'car',
       'select': [
           { 'value': 'year' },
           { 'value': 'color' },
           { 'count': 'all' }
       ],

Optional Parameters
~~~~~~~~~~~~~~~~~~~

-  *offset* (integer): defaults to 0 and is where to start indexing the
   objects from
-  *limit* (integer): defaults to 100 and can be between the values
   0-1000
-  *orderBy* (list of dictionaries): requires at at least one dictionary
   and has the form of key:value with direction being either ascending
   or descending: {‘value’: {key}, ‘direction’: asc/desc}
-  *ssearch* (string): this allows you to search any string
-  *groupBy* (list of strings): allows you to filter by 1 or more column
   names.

This query looks for how many batt logs we have per object using groupBy
objecttype and objectid and count ``{"count": "groups"}``.

.. raw:: html

   <details>

 groupBy Request

.. raw:: html

   <pre><code>
       
       {
           "from": "x_battlog",
           "select": [{"value": "objecttype"}, {"value": "objectid"}, {"count": "groups"}],
           "groupBy": ["objecttype", "objectid"]
       }   
       
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 groupBy Response

.. raw:: html

   <pre><code>
       {
       "x_battlog": [
           {
               "objectid": "bfb402e6c1a3955a495cb6b7acbb2371",
               "objecttype": "bcdEquipment",
               "count": 2766
           },
           {
               "objectid": "e704b469467483a6380aa1f9b0d358b7",
               "objecttype": "bcdEquipment",
               "count": 2765
           }
         ]
      }

       </code></pre>

.. raw:: html

   </details>

To add to our car example, we will:

-  offset the index by 5
-  limit the results to 50
-  orderBy:

   1. the year by descending
   2. color by ascending

-  ssearch by ‘cool car’

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
| IN                                   | filters if any   | {‘age’:    |
|                                      | the values in    | {‘IN’:     |
|                                      | the list are in  | [25,30] }} |
|                                      | the search       |            |
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

.. raw:: html

   <details>

 Example File: search.json

.. raw:: html

   <pre><code>
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
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Request

.. raw:: html

   <pre><code>

       curl \ 
       -H "Authorization: Basic {Authorization Token}" \
       -H "Content-Type: application/json" \
       -H "X-API-HEADER: 1" \
       -X POST --data "@search.json" '{base_url}/search' >> bcdEquipment.json
       </code></pre>

.. raw:: html

   </details>

.. raw:: html

   <details>

 Response

.. raw:: html

   <pre><code>
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
       </code></pre>

.. raw:: html

   </details>
