Static Beacon Modes
^^^^^^^^^^^^^^^^^^^

By their definition, Bluetooth Low Energy Beacons utilizing the iBeacon
or Eddystone-UID standards advertise a static, publicly discoverable
payload – Proximity UUID:Major:Minor or NamespaceID:InstanceID. This
introduces a number of security concerns, in particular:

1. The ability for someone to ‘spoof’ an beacon in order to trick an app
   into triggering an action in the wrong place. For example, a person
   may use his or her phone to advertise the exact same ID as a useful
   beacon, and trick an app into doing something in the wrong
   micro-location.

2. The ability for someone to scan for beacons and map an environment -
   allowing rival applications to utilize the micro-location information
   and serve competing content. For example, Store Owner A may gather
   all beacon IDs from his rival store, set his app to listen for these
   IDs, and incentivize people to leave his rival store when they’re
   heard.

To solve problem number one, BlueCats beacons intersperse their own
proprietary, non-static info into the stream of iBeacon advertisements,
and any device running our SDK can utilize this info to ‘verify’ that
they are indeed hearing from a BlueCats beacon and not a spoofed beacon.

However, problem number two remains a concern as long as the Bluetooth
Advertisement remains static.

Secure Mode
^^^^^^^^^^^

For a more robust level of security, we offer you the opportunity to
switch the “mode” of a BlueCats beacon, from iBeacon or UID mode to
Secure Mode or iBeacon + Secure Mode. A Secure mode broadcast does not
advertise a static identifier, but rolls its’ advertisement
periodically.

This advertisement must be decoded by the BlueCats SDK, via a
proprietary multi-step decryption procedure and assures that only
authorized apps (utilizing the BlueCats SDK and having been granted
permission) can interpret beacons and trigger micro-location based
actions.

One of the major benefits of a beacon utilizing the iBeacon standard is
the ability for iOS to listen for them while the app isn’t open and
prompt action. BlueCats Secure Mode supports similar background
capabilities in iOS if the ‘Uses Bluetooth LE Accessories’ background
mode is requested by an app. The Android SDK background capabilities do
not change whether the beacon is broadcasting in iBeacon, UID or Secure
mode.

iBeacon + Secure Mode
^^^^^^^^^^^^^^^^^^^^^

The iBeacon + Secure Mode sets the beacon to broadcast both
advertisements in iBeacon format and also BlueCats Secure format. This
is useful for a limited set of use cases where different apps may expect
a single beacon to be broadcasting in one or the other of these formats.
It is important to note that the security concerns above related to a
beacon broadcasting solely in iBeacon mode still exist when the beacon
broadcasts in iBeacon+Secure mode.
