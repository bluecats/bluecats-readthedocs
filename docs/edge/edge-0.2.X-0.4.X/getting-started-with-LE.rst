This setup guide will help you getting started with the Location Engine
(LE). There are a few settings that you should consider when setting up
the Location Engine. There are a few ways to setup the Edges: one Edge
for one LE or many Edges for one LE. Each zone you create for the Edges
can be any amount of Edges you specify.

Table of Contents:
------------------

-  `One Edge Per Location
   Engine <https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/getting-started-with-LE#1-zone-1-edge-1-le-configuration>`__
-  `Multiple Edges Per Location
   Engine <https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/getting-started-with-LE#many-zones-many-edges-1-le-configuration>`__
-  `Advanced LE
   Settings <getting-started-with-LE#advanced-location-engine-settings>`__

1 Zone, 1 Edge, 1 LE Configuration
==================================

After setting up your Beacons, it is time to set up your Edges. This
setup is for one Edge that will forward data to itself and host the
Location Engine. The Edge with the LE will forward its Zone events to
Loop or to any host server that you want.

.. raw:: html

   <p align="center">

.. raw:: html

   </p>

Setting Up Single Edge with LE
------------------------------

This setup is for 1 Edge setup with LE. Usually, this one Edge setup is
for one zone.

1.  Log into Edge Portal `192.168.8.1 <http://192.168.8.1>`__

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png
       :alt: Edge Login

       Edge Login

2.  If your Edge does not have the LE firmware yet, we need to `update
    the
    firmware <https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/edge-update-firmware>`__
    with the Location Engine

    3. Go to System > Backup/Flash Firmware
    4. We need to Flash firmware with the Edge firmware LE
    5. Do not keep settings
    6. Flash it
    7. You will have to wait and reload the page

3.  Go to BlueCats > Advanced
4.  Make sure to NOT have ‘Enable RSSI smoothing’ clicked. It should
    look like the screenshot below.

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/disable-rssi-smoothing.png
       :alt: Disable Enable RSSI Smoothing

       Disable Enable RSSI Smoothing

    ..

       NOTE: This is important for your LE setup that you do NOT have
       ‘Enable RSSI smoothing’ clicked.

5.  Go to the bottom and ‘Click Save and Apply’
6.  Go to BlueCats > Location Engine > Settings

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-Configuration-Settings.png
       :alt: LE Debug Messages

       LE Debug Messages

7.  Decay Rate and Hysteresis affect how the Location Engine gathers
    information. You can ignore them or for more information on
    `Advanced LE
    Settings <getting-started-with-LE#advanced-location-engine-settings>`__.
8.  For Identifier Type, you must pick the right ad type that the
    Location Engine will be looking for. For our case, Eddystone UID.
9.  Next, Go to BlueCats > Location Engine > Zones

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/Location-Engine-Tab.png
       :alt: Location Engine

       Location Engine

10. For the Edge MAC, because there is only 1 Edge, we can only create
    one zone for the Location Engine.

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-Zones-MAC.png
       :alt: LE Zones MAC

       LE Zones MAC

11. Go to BlueCats > Forwarding

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-Forwarding.png
       :alt: LE Forwarding

       LE Forwarding

12. Add new endpoint
13. Name it “udpLE” or “UDP Location Engine”
14. Check Enabled
15. Event type should be BLE Scan Events
16. Protocol should be UDP
17. For Host IP address put: 127.0.0.1. This allows the Edge to send its
    scanned Events to itself.
18. For the port, pick 9942

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/udp-LE.png
       :alt: UDP LE

       UDP LE

19. Change Format to: Binary
20. Click “Save and Apply”
21. Add new endpoint
22. Call it something like “LE (Zone Events)”
23. Check Enabled
24. Change event type to Location Zone Events
25. Send it to your Forwarding Endpoint of choice
26. Change the Number per interval to 1. Change Interval length to
    (seconds): 0 (for no filter). With interval of 1 and interval length
    of 0 seconds, the Edge will send everything as soon as it sees it.
27. Click Save and Apply

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-Zone-Events-Forward.png
       :alt: LE Zone Events Forward

       LE Zone Events Forward

28. Add new endpoint
29. Name it “zoneEvents” or “LE Zone Events”
30. Check Enabled
31. Event type should be BLE Zone Events
32. The adType is specified in the LE Configuration and this option will
    not affect anything. You can ignore it.
33. The protocol can be anything, but we will use HTTP as we are sending
    the zone events to Loop
34. For our Host IP address we are putting:
    shared.api.loop.bluecats.com/events Yours can be whatever IP address
    you want to send the zone events.
35. For the port, pick 443
36. Number per interval can be 1000
37. For Interval length, type: 900 seconds.
38. Click “Save and Apply”

Many Zones, Many Edges, 1 LE Configuration
==========================================

After setting up your Beacons, it is time to set up your Edges. This
setup is for many Edges sending to one Edge with the LE. The forwarding
Edges will not need the Location Engine and only forward their scanned
data to an Edge with the LE. One Edge will forward data to itself and
run the Location Engine. The Edge with the LE will forward its Zone
events to Loop or to any host server that you want.

.. raw:: html

   <p align="center">

.. raw:: html

   </p>

Setting Up Forwarding Edges
---------------------------

1.  Log into Edge Portal `192.168.8.1 <http://192.168.8.1>`__

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png
       :alt: Log in

       Log in to Edge Web Configuration

2.  Your client Edges do not need to have Location Engine as they are
    forwarding their scanned data to the Location Engine Edge.
3.  Go to BlueCats > Advanced
4.  Make sure to NOT have ‘Enable RSSI smoothing’ clicked. It should
    look like the screenshot below.

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/disable-rssi-smoothing.png
       :alt: Disable Enable RSSI Smoothing

       Disable Enable RSSI Smoothing

    ..

       NOTE: This is important for your LE setup that you do NOT have
       ‘Enable RSSI smoothing’ clicked.

5.  Go to the bottom and ‘Click Save and Apply’
6.  Go to BlueCats > Forwarding

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-Forwarding.png
       :alt: LE Forwarding

       LE Forwarding

7.  Add new endpoint
8.  Name it “udpLE” or “UDP Location Engine”
9.  Check Enabled
10. Event type should be BLE Scan Events
11. Protocol should be UDP
12. For Host IP address put something like: 192.168.11.1. This will be
    the IP address for Host Edge.
13. For the port, pick 9942

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/udp-LE.png
       :alt: UDP LE

       UDP LE

14. Change Format to: Binary
15. Click “Save and Apply”

Setting Up Edge with the LE
---------------------------

1.  Log into Edge Portal `192.168.8.1 <http://192.168.8.1>`__

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png
       :alt: Log in

       Log in to Edge Web Configuration

2.  If your Edge that is supposed to be running the LE does not have the
    LE firmware yet, we need to `update the
    firmware <https://bluecats.github.io/documentation/edge/edge-0.2.X-0.4.X/edge-update-firmware>`__
    with the Location Engine

    3. Go to System > Backup/Flash Firmware
    4. We need to Flash firmware with the Edge firmware LE
    5. Do not keep settings
    6. Flash it
    7. You will have to wait and reload the page

3.  You will need to set this Edge’s Static IP to what you were
    forwarding your data to. For our example 192.168.11.1
4.  Go to BlueCats > Advanced
5.  Make sure to NOT have ‘Enable RSSI smoothing’ clicked. It should
    look like the screenshot below.

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/disable-rssi-smoothing.png
       :alt: Disable Enable RSSI Smoothing

       Disable Enable RSSI Smoothing

    ..

       NOTE: This is important for your LE setup that you do NOT have
       ‘Enable RSSI smoothing’ clicked.

6.  Go to the bottom and ‘Click Save and Apply’
7.  Go to BlueCats > Location Engine > Settings

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-Configuration-Settings.png
       :alt: LE Debug Messages

       LE Debug Messages

8.  Decay Rate and Hysteresis affect how the Location Engine gathers
    information. You can ignore them or for more information on
    `Advanced LE
    Settings <getting-started-with-LE#advanced-location-engine-settings>`__.
9.  For Identifier Type, you must pick the right ad type that the
    Location Engine will be looking for. For our case, Eddystone UID.
10. Next, Go to BlueCats > Location Engine > Zones

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/Location-Engine-Tab.png
       :alt: Location Engine Tab

       Location Engine Tab

11. For each Zone that you want to have, put the Edge Mac you want in
    for each Zone. Each Zone you create for the Edges can be any amount
    of Edges you specify.

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-two-Zones.png
       :alt: LE Zones MAC

       LE Zones MAC

12. Go to BlueCats > Forwarding

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-Forwarding.png
       :alt: LE Forwarding

       LE Forwarding

13. Add new endpoint
14. Name it “udpLE” or “UDP Location Engine”
15. Check Enabled
16. Event type should be BLE Scan Events
17. Protocol should be UDP
18. For Host IP address put: 127.0.0.1. This allows the Edge to send its
    scanned Events to itself.
19. For the port, pick 9942

    .. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/udp-LE.png
       :alt: UDP LE

       UDP LE

20. Change Format to: Binary
21. Click “Save and Apply”
22. Add new endpoint
23. Call it something like “LE (Zone Events)”
24. Check Enabled
25. Change event type to Location Zone Events
26. Send it to your Forwarding Endpoint of choice
27. Change the Number per interval to 1. Change Interval length to
    (seconds): 0 (for no filter). > With interval of 1 and interval
    length of 0 seconds, the Edge will send everything as soon as it
    sees it.
28. Click Save and Apply

    .. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-Zone-Events-Forward.png
       :alt: UDP LE 2

       UDP LE 2

29. Add new endpoint
30. Name it “zoneEvents” or “LE Zone Events”
31. Check Enabled
32. Event type should be BLE Zone Events
33. The adType is specified in the LE Configuration and this option will
    not affect anything. You can ignore it.
34. The protocol can be anything, but we will use HTTP as we are sending
    the zone events to Loop
35. For our Host IP address we are putting:
    shared.api.loop.bluecats.com/events Yours can be whatever IP address
    you want to send the zone events.
36. For the port, pick 443
37. Number per interval can be 1000
38. For Interval length, type: 900 seconds.
39. Click “Save and Apply”

Advanced Location Engine Settings
=================================

Go to BlueCats > Location Engine > Settings.

.. figure:: https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LE-Configuration-Settings.png
   :alt: LE Debug Messages

   LE Debug Messages

The Location Engine is a high score contest between all of the zones.
**Hysteresis** defines the amount consecutive of times the Zone has to
win a contest to create a Zone Enter or Zone Exit.

In general, a higher number for Hysteresis makes it flip slower but more
reliably and the lower the number will make it easier to flip but less
reliably. A winner is only picked when an Edge detects advertisements.
Slower ad intervals make it take longer for a winner to be picked.

For example, if you have two Edges each within their own zone and a
Beacon advertising at 4 times a second. Each time an Edge hears the
Beacon the Location Engine creates a contest and sees if the Edge is a
winner of that contest. If the hystersis is 8, one Edge would need to
win 8 consecutive contests in order to create a Zone Enter or Zone Exit.
So the Edge would take at least one second for a zone event to work.

**Decay rate** is how much you weight you want each new ad to be valued
in terms of the history of the previous ads. A smaller decay rate values
the history less and values each new advertisement more. This makes it
flip more quickly. A higher decay rate and values history more. This
would cause a Zone event to flip more slowly. The range values of a
decay rate is between 0.1 to 1.

.. raw:: html

   <!-- Hysteresis JavaScript Calculator --->

..

   You can ignore Location Engine Port and Location Engine IP.

For Identifier Type, you must pick the right ad type that the Location
Engine will be looking for. This can be either:

-  ``MAC``
-  ``Eddystone UID``
-  ``iBeacon``
-  ``BT Address``
-  ``BC Id``

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-debug-device-MAC.png
   :alt: LE Debug Messages

   LE Debug Messages

In order to enable the Debug messages (which is not from the Debug tab),
check ‘Enable Debug messages’ and add the Device MAC of a Beacon. This
must be the public mac. If privacy is on, the public mac will be
different from the device mac. This will send all advertisements
received from this Beacon to the Debug Tab, which is two tabs over.

In this example, we are using the public Device MAC 7ECE86A7A437.

   For the LE Debug, never enable when sending actual data to your
   server or to Loop. Make sure to disable and save when you are done
   with the debugger.

Debug Tab
---------

The Debug Tab by default will only display the change of Zone events
while the screen is brought up. It will not show the history of a
Beacon. So in order to see an event, a new Zone event must occur while
the screen is up.

The ID filter, will filter the device MAC that you want to see. In this
example, we are using the public Device MAC 7ECE86A7A437.

Under zones in this example, we have two Zones: Zone1 and Zone2. The
numbers underneath is the score to determine what the winner will be.
The winner had not been picked yet.

Under the Readers, there are four numbers in this order are: rssi (-77),
smooth rssi (-77), measured power (-56), approximate distance (11.2), ad
count (2).

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/LE-advanced-debug.png
   :alt: LE Debug Tab

   LE Debug Tab
