Loop is a Cloud Platform that comes from the phrase OODA Loop. OODA Loop
is a decsion cycle developed by the military that stands for *Observe*,
*Orient*, *Decide*, *Act*. The Loop Cloud Platform enables you to manage
the relationship between your assets and staff with real-time device
management and analytics, transforming your location data into
meaningful observations.

How It Works
------------

BlueCats manufactures one-way transmitter devices called Beacons that
utilize Bluetooth Low Energy (BLE) technology. Beacons use constant
transmissions to add data to real-world locations and objects. BLE
gateways called Edge Relays are setup with a Location Engine to scan for
Beacons and then send that scanned Beacon data to Loop. Loop takes your
scanned Beacon Data and can assign Object Types to the things your
company cares about like people and assets. Loop can also take Beacon
data and can report the data and use state logic to make decisions about
different types of events.

Objects
-------

Object Types are user defined such as Vehicles, People, Equipment or
Places. For example, an objectType of *Vehicle* can be assigned to a
Beacon with the name “ServiceTruck1”. This can then give you
significance to your objects and when they trigger events. You can
currently use an EventID to assign an object to one Beacon or Edge. Each
objectType can hold numerous field types:

-  Strings
-  Integers
-  Links to other objects

Example of an Object of Vehicle:

::

           "Vehicle": [
               {
                   "ID": "1bb123feca2dc5bf2e1204f3bf7e132b",
                   "GroupType": "Customer",
                   "GroupName": "Company Car Lot",
                   "EventID": "eddyUID#314F6D1A2C123455FA6512300001FC51",
                   "type": "Truck",
                   "make": "Ford",
                   "model": "F-150",
                   "VIN": null,
                   "licensePlateNumber": null,
                   "year": "2014",
                   "color": "White",
                   "owner": "George Carlin",
                   "batterySoC": 58,
                   "batterySoCObservedAt": "2017-10-13T21:07:11.289795",
                   "temp": null,
                   "tempObservedAt": null,
                   "impactMagnitude": null,
                   "impactObservedAt": null,
                   "place": null,
                   "placeChangedAt": null,
                   "long": -97.723419,
                   "lat": 30.259186,
                   "positionObservedAt": "2017-10-13T21:07:11.289795"
               },
    

When you initialize your Loop stack, you will specify the number and
type of objects that will populate your Loop stack. After you initialize
Loop, you can always add objects later as you see fit. Once those
objects have been assigned, those objects can be configured to trigger
events.

Events
------

Events are assigned to objects so that a so that a specific rule can
trigger a change in an object value. Once those rules have been
assigned, those objects can trigger events and update values through
*Zone Events* and *Value Events*.

*Zone Events*
~~~~~~~~~~~~~

Zone Events modify a value for an object whenever a specific Zone is
entered or exited a value. Zone Events can be entering a *Place* with
significant GPS coordinates such as a Worksite or Office. For Zone
Events, you will receive a Zone from a Scan Event from our BLE Scanner
(Edge Relay). State Logic will be applied to the Scan Event. The State
Logic will take the Zone Name, the Key, and the value in the object to
be changed. The value that is changed will be timestamped.

*Value Events*
~~~~~~~~~~~~~~

Value Events modify a value for an object whenever a another value
changes or reaches a specific value on that object. Value Events can be
a sensor Beacon recording a specific temperature or impact reading. For
Value Events, you will receive a value from a Scan Event from our BLE
Scanner (Edge Relay). State Logic will be applied to the Scan Event. The
State Logic will take the value that changed from Scan Event, and the
value in the object to be changed. The value that is changed will be
timestamped.

Groups and Users
----------------

After creating your Loop Stack, you can create Groups and Users with
different permissions. Each group can then contain subgroups that hold
their own objects. A company that has subcompanies

What You Can Observe
~~~~~~~~~~~~~~~~~~~~

You will be able to have a list view of all of your objects. These
objects will have detailed views, which will have more specfied data
reported. Depending on the type of data the object will report different
type of figures. If it is Beacon with:

-  sensor data -> a graph
-  location data -> a map

Evantually, object reports, will also be able to return the recorded
event history and state changes of those objects.

.. raw:: html

   <!---
   - Events
       - Heartbeat
       - Scan
   - Where's Ronaldo? 
   --->
