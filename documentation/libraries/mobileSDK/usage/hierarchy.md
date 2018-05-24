---
layout: documentation
title: Hierarchy
---

### Teams
A Team is the topmost identifier in the BlueCats hierarchy. Teams can be used to separate beacons by client, brand or into non-related beacon networks. Every team has an owner with advanced management capabilities over the team.

Other than Teammates, all entities in the BlueCats platform belong to a specific team. Information can be shared across teams in a variety of ways, but entities such as Apps, Sites and Categories are contained within a particular team.

### Apps
Android, iOS and API client applications are represented by Apps. Every app is assigned an app token or client ID to associate it with a particular BlueCats Team. Create an app for every application that will be accessing your beacon network. Insights, or beacon visits, are sortable by app and network access can be granted to specific apps from other teams.

### Teammates
Teammates are BlueCats management system users. Teammates can act as contributors or consumers. Contributors can manage beacon settings, create and alter categories and sites, and invite other teammates. Consumers have limited access rights to view Insights and update beacons with BlueCats Reveal. Certain management actions, such as deleting a category, can only be performed by a team's owner.

### Sites
Groups of beacons that share a common geo-location are located in a single site. Sites can contain a postal address or GPS coordinates and maps. In addition to helping the SDK with the caching of Beacon hierarchical data, sites can contain custom values (key-value pairs) and can be used to trigger macro-location entry and exit notifications.

Sites within your BlueCats network can be marked private or public. Beacon metadata in private sites can only be used by apps in your team or by other teams to which you have explicitly granted access. Metadata from beacons in public sites is accessible to other BlueCats platform users unless the beacons are in a non-public advertising mode (i.e. Secure Mode).

### Categories
Categories are used to add high-level context to multiple beacons across an entire team. They can be used to notify your app of valuable location information such as 'Entrance', 'Endcap' or 'Menswear'.

### Zones
Zones are dynamic groupings of beacons that can be composed of multiple categories, sites or individual beacons. Zones are used to trigger entry, exit, and dwell time events within a site across your entire team.

### Maps
Maps are layouts or floor plans assigned to a site. Maps can be used for deployment mapping, rendering user location and heat mapping. Beacons are added to maps via the web app and can help when configuring beacon settings by estimating beacon range and the number of beacons needed to cover a location.

### Regions
Regions define the available iBeacon and/or Eddystone-UID values that are available to your beacons. Regions can be used for organization or to enhance beacon detection in iOS when a app is closed.

### Beacons
Beacons are...beacons! Beacons can only be assigned a single site, team and map, but can be assigned multiple categories and can carry individual data in the form of custom values.
