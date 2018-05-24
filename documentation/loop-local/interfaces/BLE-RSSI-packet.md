---
layout: documentation
title: BLE RSSI Packet
---



-------
# Location Engine

The Location Engine is a Windows Service that listens for RSSI reading packets from BLE Scanner Device sent over UDP.  From this data, it will resolve device locations and relay that data to the Distributor. 

Listens on UDP Port: `58327`

## Packet Protocol


### RSSI Packet: Packet Type 0x0003

#### Header

Offset  | Length | Type      | Field
--------|--------|-----------|-------
0       | 2      | uint16_t  | Packet Type
2       | 6      | uint8_t[] | BLE Scanner MAC
8       | 6      | uint8_t[] | BLE Device MAC
14      | 1      | uint8_t   | BLE Device Type
15      | 1      | int8_t    | RSSI
16      | 1      | int8_t    | Measured Power
17      | 1      | uint8_t   | Sequnce Number
18      | 1      | uint8_t   | BLE Channel
19      | 1      | uint8_t   | Battery Level
20      | 1      | uint8_t   | Payload Length
21      | 1-255  | uint8_t[] | Payload Data

### Heartbeat Packet: Packet Type 0x0003

#### Header

Same structre with RSSI package type. 

Offset  | Length | Type      | Field
--------|--------|-----------|-------
0       | 2      | uint16_t  | Packet Type
2       | 6      | uint8_t[] | BLE Scanner MAC
8       | 6      | uint8_t[] | 000000000000
14      | 1      | uint8_t   | 00
15      | 1      | int8_t    | 00
16      | 1      | int8_t    | 00
17      | 1      | uint8_t   | 00
18      | 1      | uint8_t   | 00
19      | 1      | uint8_t   | 00
20      | 1      | uint8_t   | Payload Length
21      | 1-255  | uint8_t[] | Payload Data

#### Payload 

The payload can contain any number of items, up to the max payload length of 255. Below is the format for a single item. 

Offset  | Length | Type      | Field
--------|--------|-----------|-------
0       | 1      | uint8_t   | Data Type
1       | 1      | uint8_t   | Length
2       | 1 - n  | uint8_t[] | Data

>*fields are little-endian

Payload Data Type Definitions.

Data Type | Length   | Description        | Format
----------|----------|--------------------|---------
0x01      | 6        | Bluetooth Address  | uint8_t[]
0x02      | 1-16     | BlueCats Identifier| uint8_t[]
0x03      | 20       | iBeacon Identifier | uint8_t[]
0x04      | 16       | Eddystone UID      | uint8_t[]
0x05      | 1-17     | Eddystone Url      | uint8_t[]
0x06      | 4        | Advert Mask        | uint32_t
0x07      | 4        | Temperature        | float
0x08      | 4        | Accelerometer X    | float
0x09      | 4        | Accelerometer Y    | float
0x0A      | 4        | Accelerometer Z    | float
0x0B      | 1        | Settings Version   | uint8_t
0x0C      | 4        | FW UID         | uint8_t[]
0x0D      |1-16      | Edge FW Version    | uint8_t[]
0x0E      |4         | Edge Uptime(seconds)|uint32_t(little-endian)
0x0F      |1         | Edge Memory Free(%)| uint8_t
0x10      |1         | Edge CPU Idle(%)   | uint8_t
0x11      |2         | Heartbeat Duration(sec) | uint16_t(little-endian)
0x12      |4         | BLE packet count in duration   | uint32_t(little-endian)
0x13      |4         | Filtered BLE packet count in duration | uint32_t(little-endian)