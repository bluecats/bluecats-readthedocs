---
layout: documentation
title: Events
category: Loop
order: 3
---


## Incoming Event Types

| Type    | Description |
|---------------|----------------------------------|
| *bleScan* | ble beacon/tag scanned | 
| *nfcScan* | nfc tag scanned |
| *rfidScan* | rfid tag scanned |
| *barScan* | barcode scanned |
| *idScan* | identifier scannned |
| *enZone* | entered zone |
| *exZone* | exited zone |
| *chZone* | change zone |
| *chgCtrlHB* | charge controller heartbeat |
| *wwanHB* | cellular heartbeat |
| *gpsHB* | gps heartbeat |
| *edgeHB* | edge relay heartbeat |
| *clientHB* | client heartbeat |
| *legPos* | PLUS location engine position |
| *legHB* | PLUS location engine heartbeat |
| *clientPos* | BC Loop client position |
| *log* | object activity log |
| *scanEvent* | *deprecated* |
| *reportEvent* | *deprecated* |

## Internal Event Types

| Type    | Description |
|---------------|----------------------------------|
| *objChanged* | object changed |
| *chPlace* | change place |

## Event Identifiers

| Identifier | Description |            
|------------------|------------------------------|
| *clientID*       | bc loop client id |
| *eddyUID*        | eddystone uid  |
| *btAddr*         | Bluetooth address |
| *serNum*         | Serial number |
| *iBeac*          | iBeacon |
| *bar*            | Barcode |
| *nfc*            | NFC tag id |
| *rfid*           | RFID tag id |
| *uwb*            | UWB tag id |
| *barcode*        | Barcode |
| *edgeMAC*        | Edge mac |
| *loopObjID*      | BC Loop object id |
| *plusEntityID*   | PLUS entity id |
| *identifier*     | {idKey}#{idVal} |
| *identifiers*    | array of identifiers |

## Measurement Keys

| Key | Description |            
|------------------|------------------------------|
| *vBatt*       | battery voltage |
| *pBatt*       | battery state of charge (percentage) |
| *temp*        | temperature (celsius) |
| *accX*        | accelerometer x |
| *accY*        | accelerometer y |
| *accZ*        | accelerometer z |
| *impMag*      | impact magnitude |
| *impTime*     | impact time |
| *hum*    	| humidity |
| *press*    	| pressure |
| *gas*         | gas or volatile organic compounds |
| *uptime*      | time in seconds since boot |

## Location Providers

| Value | Description  |
|---------------|------------------|
| *plus.leg* | PLUS location engine |
| *edge.leg* | BC Edge location engine |
| *edge.gps* | BC Edge GPS |
| *android.{android_provider}* | Android location provider, GPS or network are typical values |
| *ios.{ios_provider}* | iOS location provider, GPS or network are typical values  |
| *loop.solver* | BC Loop position solver  |

## Location

### Location Keys 

| Key | Description |
|---------------|----------------------------------|
| *zone* | special area |
| *zones* | array of current zone and previous zone |
| *gps* | latitude, longitude, ... |
| *ipAddr* | public IP Address (server side |
| *mapPoint* | point on map |

### Zone Keys

| Key | Description |
|---------------|----------------------------------|
| *id* | zone ID |
| *name* | zone name |
| *type* | static or .... |
| *teamID* | BC device managment team ID |
| *siteID* | BC device managment site ID |
| *mapID* | BC device managment map ID |
| *prov* | location provider |

### GPS Keys

| Key | Description |
|----------------|----------------------------------|
| *ts*           | fix timestamp, use system time
| *lat*          | latitude in decimal degrees |
| *long*          | longitude in decimal degrees |
| *alt*          | altitude in meters |
| *eph*          | horizontal error estimate in meters |
| *epx*          | longitude error estimate in meters |
| *epy*          | latitude error estimate in meters |
| *epv*          | vertical error estimate in meters |
| *track*        | course over ground in degrees from true northcalled bearing in Android or course in iOS|
| *epd*          | direction error estimate in degrees |
| *spd*          | speed over ground in meters/second |
| *eps*          | speed error estimate in meters/second |
| *conf*         | confidence interval for error estimates/probabilities |
| *prov*         | location provder |
| *status*       | GPS status, 2=DGPS fix|
| *mode*         | NMEA mode, 0=no mode value yet seen, 1=no fix, 2=2D, 3=3D |
| *climb*        | climb is positive, sink is negative in meters/second |
| *epc*          | climb/sink error estimate in meters/second |

### Map Point Keys

#### Cartesian Coordinate System

| Keys  | Description |
|----------------|----------------------------------|
| *coordSys* | "xyz" |
| *x* | x in meters |
| *y* | y in meters |
| *z* | z in meters |
| *teamID* | BC device managment team ID |
| *siteID* | BC device managment site ID |
| *mapID* | BC device managment map ID |
| *prov* | location provder |

#### Geographic Coordinate System

| Keys  | Description |
|----------------|----------------------------------|
| *coordSys* | "geo" |
| *lat* | latitude in decimal degrees |
| *long* | longitude in decimal degrees |
| *alt* | altitude in meters |
| *teamID* | BC device managment team ID |
| *siteID* | BC device managment site ID |
| *mapID* | BC device managment map ID |
| *prov* | location provder |


## Incoming Event Formats

### BC Loop Client Events

#### Client Position Event 

```
{
   "type":"clientPos",
   "clientID|edgeMAC":"{id}",
   "gps":{
      "ts":"2017-10-20T10:34:48.000Z",
      "lat":46.498293369,
      "long":7.567411672,
      "alt":1343.127,
      "eph":36.000,
      "epx":36.000,
      "epy":32.321,
      "track":10.3788,
      "epd":,
      "spd":0.091,
      "eps":,
      "conf":68,
      "prov":"edge.gps",
   },
   "ts":"2017-10-20T10:34:49.000Z"
}
```
#### Client Heartbeat Event 

```
{
   "type":"clientHB",
   "clientID":"{id}",
   "appVer":"3.2.1",
   "gpsMode":"gps_only|high_acc|batt_savings",
   "reachability":"wifi|wwan|eth",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

### Scan Events

#### Barcode Scanned Event 

```
{
   "type":"barScan",
   "identifier":"bar#1234567890",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### NFC Tag Scanned Event 

```
{
   "type":"nfcScan",
   "identifier":"nfc#1234567890",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### RFID Tag Scanned Event 

```
{
   "type":"rfidScan",
   "identifier":"rfid#1234567890",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### BLE Scan Event 

```
{
   "type":"bleScan",
   "identifiers":["bcID#100000000","iBeac#00000000000000000000000000000000","eddyUID#00000000000000000000000000000000"],
   "rssi":-85,
   "mPow":-65,
   "gps":{
      "ts":"2017-10-20T10:34:48.000Z",
      "lat":46.498293369,
      "long":7.567411672,
      "alt":1343.127,
      "eph":36.000,
      "epx":36.000,
      "epy":32.321,
      "track":10.3788,
      "epd":,
      "spd":0.091,
      "eps":,
      "conf":68,
      "prov":"android.gps",
   },
   "meas":{
      "vBatt":2.90,
      "pBatt":95,
      "temp":45.5,
      "accX":1.0,
      "accY":1.0,
      "accZ":1.0,
      "uptime":200
   },
   "ts":"2017-10-20T10:34:49.000Z"
}
```
```
{
   "type":"bleScan",
   "eddyUID":"00000000000000000000000000000000",
   "rssi":-85,
   "mPow":-65,
   "gps":{
      "ts":"2017-10-20T10:34:48.000Z",
      "lat":46.498293369,
      "long":7.567411672,
      "alt":1343.127,
      "eph":36.000,
      "epx":36.000,
      "epy":32.321,
      "track":10.3788,
      "epd":,
      "spd":0.091,
      "eps":,
      "conf":68,
      "prov":"edge.gps"
   },
   "meas":{
      "vBatt":2.90,
      "pBatt":95,
      "temp":45.5,
      "accX":1.0,
      "accY":1.0,
      "accZ":1.0,
      "uptime":200
   },
   "ts":"2017-10-20T10:34:49.000Z"
}
```

### Zone Events 

#### Entered Zone Event 

```
{
   "type":"enZone",
   "identifiers":["uwb#00000000","iBeac#00000000000000000000000000000000","eddyUID#00000000000000000000000000000000"],
   "zone":{
      "name":"",
      "id":"",
      "type":"static",
      "mapID":"",
      "prov":"edge.leg"
   },
   "gps":{},
   "meas":{
       "vBatt":,
       "pBatt":,
       "temp":,
       "accX":,
       "accY":,
       "accZ":,
       "upime":200
    },
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### Exited Zone Event 

```
{
    "type":"exZone",
    "identifier":"uwb#00000000",
    "zone":{
       "name":"",
       "id":"",
       "type":"static",
       "mapID":"",
       "prov":"edge.leg"
    },
    "meas":{
       "vBatt":,
       "pBatt":,
       "temp":,
       "accX":,
       "accY":,
       "accZ":,
       "upime":200
    },
    "ts":"2017-10-20T10:34:49.000Z"
}
```

#### Changed Zone Event 

```
{
   "type":"chZone",
   "identifier":"eddyUID#00000000000000000000000000000000",
   "zones":[{
      "name":"",
      "id":"",
      "type":"static",
      "mapID":""
   },{
      "name":"",
      "id":"",
      "type":"static",
      "mapID":""
   }],
   "meas":{
       "vBatt":,
       "pBatt":,
       "temp":,
       "accX":,
       "accY":,
       "accZ":,
       "upime":200
    },
   "ts":"2017-10-20T10:34:49.000Z"
}
```

### PLUS Location Engine Events 

#### LEG Position Event

```
{
   "type":"legPos",
   "identifiers":["uwb#00000000","iBeac#00000000000000000000000000000000","eddyUID#00000000000000000000000000000000"],
   "data":{"vBatt":,"pBatt":,"temp":,"accX":,"accY":,"accZ":,"bTime":"Z"},
   "mapPoint":{
      "coordSys":"geo",
      "lat":,
      "long":,
      "alt":,
      "teamID":"",
      "siteID":"",
      "mapID":"",
      "prov":"plus.leg"
   },
   "ts":"2017-10-20T10:34:49.000Z"
}
```

### Edge Heartbeat Events 

#### Charge Controller Heartbeat Event 

```
{
   "type":"chgCtrlHB",
   "clientID|edgeMAC":"{id}",
   "v":12670,
   "i":-50,
   "il":-50,
   "cs":3,
   "ppv":2,
   "vpv":30110,
   "err":0,
   "fw":116,
   "pid":41026,
   "load":1,
   "h20":8,
   "h21":49,
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### WWAN Heartbeat Event 

```
{
   "type":"wwanHB",
   "clientID|edgeMAC":"{id}",
   "mfr":
   "rev":
   "imei":
   "uptime":292,
   "temp":41,
   "rssi":-47,
   "rsrp":-71,
   "rsrq":-8,
   "sinr":19.0,
   "ecio":,
   "txPow":0
   "band":"B13", 
   "cellID":"02173403 (35075075",
   "lteBW":"10 MHz",
   "lteRxChan":5230,
   "lteTxChan":23230,
   "mode":"ONLINE",
   "sMode":"LTE",
   "psState":"Attached",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### GPS Heartbeat Event 

```
{
   "type":"gpsHB",
   "clientID|edgeMAC":"{id}",
   "gps":{},
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### Edge Heartbeat Event 

```
{
   "type":"edgeHB",
   "clientID|edgeMAC":"{id}",
   "fwVer":"0.3.0",
   "fwUID":9,
   "uptime":502769,
   "fram":27189248,
   "bleMods":[{"fwVer":"0240","fwUID":50000073,"uptime":432168,"bCount":0}],
   "interfaces":[{"name":"wwan0","ip":"100.92.208.205"},{"name":"br-lan","ip":"192.168.11.1"}],
   "defaultIP":"100.110.90.2",
   "ts":"2017-10-20T10:34:49.000Z"
}
```

## Internal Event Formats

### Object Events

#### Object Changed Event

```
{
   "type":"objChanged",
   "chKeys":["key1","key2",...],
   "obj":{},
   "ts":"2017-10-20T10:34:49.000Z"
}
```

#### Change Place Event

```
{
   "type":"chPlace",
   "objType":"",
   "objID":"",
   "placeID":"",
   "ts":"2017-10-20T10:34:49.000Z"
}
```
