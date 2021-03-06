Table of Contents
-----------------

1. `Firmware Release Information <#firmwareRelease>`__
2. `Serial Packet Format <#packetFormat>`__
3. `Data Types <#dataTypes>`__
4. `Serial Configuration <#serialConfig>`__
5. `Commands <#commands>`__

Firmware Release Information
----------------------------

The API calls referenced in this document relate to BlueCats firmware
release xx.xx.xx installed on BLE modules BC010/BC011.

Published xx March 2017.

Serial Packet Format
--------------------

Commands are sent from host to module over a serial connection (SPI, I2C
or DIO).

For each command, the beacon provides a corresponding response. Each
field in the response packet arrives over serial with most significant
byte first (big-endian), for easier readability.

The response payload’s first (or sometimes only) byte is a response
code.

The first 6 bytes of each PDU is the header. The payload is whatever
follows.

Command
~~~~~~~

+------+----------------+-----------------+
| Byte | Length         | Description     |
+======+================+=================+
| 0    | 1 byte         | Message type    |
+------+----------------+-----------------+
| 1    | 1 byte         | Class ID (0xBC) |
+------+----------------+-----------------+
| 2    | 1 byte         | Command ID      |
+------+----------------+-----------------+
| 3    | 1 byte         | Payload length  |
+------+----------------+-----------------+
| 4    | 1 byte         | Payload CRC8    |
+------+----------------+-----------------+
| 5    | 1 byte         | Header CRC8     |
+------+----------------+-----------------+
| 6-n  | 0 to 255 bytes | Payload         |
+------+----------------+-----------------+

Command IDs:
~~~~~~~~~~~~

.. raw:: html

   <table>

.. raw:: html

   <tr>

.. raw:: html

   <th>

ID

.. raw:: html

   </th>

.. raw:: html

   <th>

Short Description

.. raw:: html

   </th>

.. raw:: html

   <th>

Long Description

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x01

.. raw:: html

   </td>

.. raw:: html

   <td>

Meow

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x02

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Bluetooth Address

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x03

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Firmware Version

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x04

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Firmware UID

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x05

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Model Number

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x06

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Status

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x07

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Encrypted Status

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x08

.. raw:: html

   </td>

.. raw:: html

   <td>

Start Scan

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x09

.. raw:: html

   </td>

.. raw:: html

   <td>

Stop Scan

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0A

.. raw:: html

   </td>

.. raw:: html

   <td>

Write Firmware Header

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0B

.. raw:: html

   </td>

.. raw:: html

   <td>

Write Firmware Block

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0C

.. raw:: html

   </td>

.. raw:: html

   <td>

Go DFU

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0D

.. raw:: html

   </td>

.. raw:: html

   <td>

Write BLE Response

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0E

.. raw:: html

   </td>

.. raw:: html

   <td>

Read Serial API Version

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x0F

.. raw:: html

   </td>

.. raw:: html

   <td>

Start PWM

.. raw:: html

   </td>

.. raw:: html

   <td>

{Frequency, 0-15, (steps of 80 Hz, 1kHz - 2.2 KHz) byte}, {duration
seconds, byte}

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x10

.. raw:: html

   </td>

.. raw:: html

   <td>

Stop PWM

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x42

.. raw:: html

   </td>

.. raw:: html

   <td>

Connect

.. raw:: html

   </td>

.. raw:: html

   <td>

Connect to BLE device connection

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x43

.. raw:: html

   </td>

.. raw:: html

   <td>

Login

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x44

.. raw:: html

   </td>

.. raw:: html

   <td>

Update Settings

.. raw:: html

   </td>

.. raw:: html

   <td>

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x45

.. raw:: html

   </td>

.. raw:: html

   <td>

Disconnect

.. raw:: html

   </td>

.. raw:: html

   <td>

Disconnect from BLE device connection

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x46

.. raw:: html

   </td>

.. raw:: html

   <td>

Write Firmware Opcode

.. raw:: html

   </td>

.. raw:: html

   <td>

writes to control char

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

0x47

.. raw:: html

   </td>

.. raw:: html

   <td>

Write Firmware Bulk Data

.. raw:: html

   </td>

.. raw:: html

   <td>

writes to gravy char

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </table>

Response
^^^^^^^^

+-----------------------+-----------------------+-----------------------+
| Byte                  | Length                | Description           |
+=======================+=======================+=======================+
| 0                     | 1 byte                | `Message              |
|                       |                       | type <#messageType>`_ |
|                       |                       | _                     |
+-----------------------+-----------------------+-----------------------+
| 1                     | 1 byte                | Class ID (0xBC)       |
+-----------------------+-----------------------+-----------------------+
| 2                     | 1 byte                | Message ID            |
+-----------------------+-----------------------+-----------------------+
| 3                     | 1 byte                | Payload length **>    |
|                       |                       | 0x00**                |
+-----------------------+-----------------------+-----------------------+
| 4                     | 1 byte                | Payload CRC8          |
+-----------------------+-----------------------+-----------------------+
| 5                     | 1 byte                | Header CRC8           |
+-----------------------+-----------------------+-----------------------+
| 6-n                   | 1 byte                | Payload starts with   |
|                       |                       | \ `Command Response   |
|                       |                       | Code <#responseCode>` |
|                       |                       | __                    |
+-----------------------+-----------------------+-----------------------+

Response Codes
^^^^^^^^^^^^^^

Message Type Field
^^^^^^^^^^^^^^^^^^

The message type is used to help determine if the data represents a
command or response host initiated communication to the BLE module
(0x00), or an event sent from the BLE module (0x80).

+---------------------+------+
| Value               | ID   |
+=====================+======+
| Command or Response | 0x00 |
+---------------------+------+
| Event               | 0x80 |
+---------------------+------+

Responses
---------

Command response IDs:
~~~~~~~~~~~~~~~~~~~~~

+--------+---------------------------------------+---------------------+
| ID     | Meaning                               | Description         |
+========+=======================================+=====================+
| 0x00   | Success                               | Command succeeded.  |
+--------+---------------------------------------+---------------------+
| 0x01   | Busy                                  | Previous command    |
|        |                                       | needs to complete   |
|        |                                       | or be canceled      |
|        |                                       | before sending next |
|        |                                       | command.            |
+--------+---------------------------------------+---------------------+
| 0x02   | Advertising Disabled                  | Send command only   |
|        |                                       | when advertising    |
|        |                                       | enabled.            |
+--------+---------------------------------------+---------------------+
| 0x03   | Buffer Overflow                       | Buffer overflowed.  |
+--------+---------------------------------------+---------------------+
| 0x04   | Remote Hung Up                        | Command not         |
|        |                                       | available when no   |
|        |                                       | device is           |
|        |                                       | connected.          |
+--------+---------------------------------------+---------------------+
| 0x05   | Gatt Write                            | Failed to write     |
|        |                                       | response to BLE     |
|        |                                       | data request.       |
+--------+---------------------------------------+---------------------+
| 0x06   | Invalid Parameter                     | Parameter in        |
|        |                                       | command payload is  |
|        |                                       | invalid.            |
+--------+---------------------------------------+---------------------+
| 0x07   | Not Supported                         | Command not         |
|        |                                       | supported in this   |
|        |                                       | firmware version.   |
+--------+---------------------------------------+---------------------+
| 0x08   | No Pending Changes                    | Send begin setting  |
|        |                                       | update command      |
|        |                                       | before end settings |
|        |                                       | update command.     |
+--------+---------------------------------------+---------------------+
| 0x09   | Header CRC failure                    | Header CRC check    |
|        |                                       | failed.             |
+--------+---------------------------------------+---------------------+
| 0x0A   | Payload CRC failure                   | Payload CRC check   |
|        |                                       | failed.             |
+--------+---------------------------------------+---------------------+
| 0x0B   | Firmware Update Failed                |                     |
+--------+---------------------------------------+---------------------+

Event IDs:
~~~~~~~~~~

+--------+---------------------------------------+---------------------+
| ID     | Short Description                     | Long Description    |
+========+=======================================+=====================+
| 0x01   | BLE Session Connected                 |                     |
+--------+---------------------------------------+---------------------+
| 0x02   | BLE Session Disconnected              |                     |
+--------+---------------------------------------+---------------------+
| 0x03   | BLE Debug Mode                        |                     |
+--------+---------------------------------------+---------------------+
| 0x04   | BLE Auth Succeeded                    |                     |
+--------+---------------------------------------+---------------------+
| 0x05   | BLE Auth Failed                       |                     |
+--------+---------------------------------------+---------------------+
| 0x06   | BLE Event Error                       |                     |
+--------+---------------------------------------+---------------------+
| 0x07   | BLE Settings Saved                    |                     |
+--------+---------------------------------------+---------------------+
| 0x08   | BLE Data Requested                    |                     |
+--------+---------------------------------------+---------------------+
| 0x09   | BLE Data Blocks Sent                  |                     |
+--------+---------------------------------------+---------------------+
| 0x0A   | BLE Data Indicated                    |                     |
+--------+---------------------------------------+---------------------+
| 0x0B   | BLE Scanning Started                  |                     |
+--------+---------------------------------------+---------------------+
| 0x0C   | BLE Scanning Stopped                  |                     |
+--------+---------------------------------------+---------------------+
| 0x0D   | BLE Device Ranged                     |                     |
+--------+---------------------------------------+---------------------+
| 0x0E   | BLE Device Entered                    |                     |
+--------+---------------------------------------+---------------------+
| 0x0F   | BLE Device Exited                     |                     |
+--------+---------------------------------------+---------------------+
| 0x10   | BLE Device Discovered                 |                     |
+--------+---------------------------------------+---------------------+
| 0x11   | BLE Module Boot                       |                     |
+--------+---------------------------------------+---------------------+
| 0x84   | Unrecognized Command                  | Command was not     |
|        |                                       | recognized          |
+--------+---------------------------------------+---------------------+
| 0x85   | Permission Denied                     | Permission has been |
|        |                                       | disabled or         |
|        |                                       | rejected for format |
|        |                                       | device              |
+--------+---------------------------------------+---------------------+
| 0x86   | UART Read Failure                     | Error during UART   |
|        |                                       | communication       |
+--------+---------------------------------------+---------------------+

Data Types
----------

Some payloads will include multiple data types. For instance if you are
in scanning mode, a temperature measurement ad can include negative
numbers.

+-----------------+-----------------+-----------------+-----------------+
| Type            | Description     | Example (Human  | Example (Hex)   |
|                 |                 | readable)       |                 |
+=================+=================+=================+=================+
| int8            | Signed integer  | -42             | 0xD6            |
|                 | stored in one   |                 |                 |
|                 | byte 2’s        |                 |                 |
|                 | complement form |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| uint8           | Unsigned        | 0x42            | 0x2A            |
|                 | integer stored  |                 |                 |
|                 | in 1 byte       |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| uint16          | Unsigned        | 1701            | 0xa5 0x06       |
|                 | integer stored  |                 |                 |
|                 | in 2 bytes      |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| uint32          | Unsigned        | 1000000         | 0x40 0x42 0x0f  |
|                 | integer stored  |                 | 0x00            |
|                 | in 4 bytes      |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| uint8array      | Byte array      | “Hello”         | 0x68 0x65 0x6c  |
|                 |                 |                 | 0x6c 0x6f       |
+-----------------+-----------------+-----------------+-----------------+

Serial Configuration
--------------------

+-----------+-------------------------------+
| Setting   | Value                         |
+===========+===============================+
| Handshake | RequestToSend                 |
+-----------+-------------------------------+
| BaudRate  | 921600 baud (BC01X Ti Module) |
+-----------+-------------------------------+
| DataBits  | 8                             |
+-----------+-------------------------------+
| StopBits  | One                           |
+-----------+-------------------------------+
| Parity    | None                          |
+-----------+-------------------------------+

..

   Future release of this device will enable speed ranges from 115200 -
   921600. Lower speeds would present a issue

Commands
--------

Calculating CRC
~~~~~~~~~~~~~~~

We use CRC’s to ensure that the high speed of our serial bursts isn’t
causing data integrity issues (Serial protocols are allowed a small
percentage of errors). CRCs help determine whether your received or sent
data is in good order.

During testing it’s handy to be able to send and receive sample commands
and data. The commands shown below include CRC8s for the headers and
payload, however if you are sending alternate payloads your code will
need to calculate the CRC8 (8 bit CRC).

.. code:: c

   //
   //  main.c
   //  CRC_Sampler
   //
   //  Usage: Enter your hex string as concatenated bytes.
   //  E.G. 0011AB
   //

   #include <stdio.h>
   #include <stdlib.h>

   int main(int argc, const char * argv[]) {
       const char *hexData = argv[1];
       
       unsigned long length = strlen(hexData)/2;
       uint8_t pduBuf[(int)length];
           
       size_t count = 0;
       printf ("Parsing:");
       for(count = 0; count < sizeof(pduBuf)/sizeof(pduBuf[0]); count++) {
           sscanf (hexData, "%02hhx", &pduBuf[count]);
           printf (" %02X", pduBuf[count]);
           hexData += 2;
       }
       printf ("\n");
       
       const uint8_t *data = pduBuf;
       
       unsigned crc = 0;
       int i, j;
       for (j = (int)length; j; j--, data++) {
           crc ^= (*data << 8);
           for(i = 8; i; i--) {
               if (crc & 0x8000)
                   crc ^= (0x1070 << 3);
               crc <<= 1;
           }
       }
       printf ("Calculated CRC: %02hhX\n", (uint8_t)(crc >> 8));

       return 0;
   }

Meow
~~~~

Send this command to reset your module.

.. _command-1:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x01  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0x1B  | Header CRC8    |
+------+-------+----------------+

.. _response-1:

Response
~~~~~~~~

+------+-------+-----------------------+
| Byte | Value | Description           |
+======+=======+=======================+
| 0    | 0x00  | Message type          |
+------+-------+-----------------------+
| 1    | 0xBC  | Class ID              |
+------+-------+-----------------------+
| 2    | 0x01  | Message ID            |
+------+-------+-----------------------+
| 3    | 0x00  | Payload Length        |
+------+-------+-----------------------+
| 4    | 0x00  | Payload CRC8          |
+------+-------+-----------------------+
| 5    | 0x05  | Header CRC8           |
+------+-------+-----------------------+
| 6    | 0x00  | Command Response Code |
+------+-------+-----------------------+

Read BT Address
~~~~~~~~~~~~~~~

Send this command to retrieve the Bluetooth MAC Address of the serial
beacon.

.. _command-2:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x02  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0xA6  | Header CRC8    |
+------+-------+----------------+

.. _response-2:

Response
~~~~~~~~

*Example Response:* **00 BC 02 07 8D 67 00 98 07 2D 05 FE 54**

+--------+-------+--------------------------------+
| Byte   | Value | Description                    |
+========+=======+================================+
| 0      | 0x00  | Message type                   |
+--------+-------+--------------------------------+
| 1      | 0xBC  | Class ID                       |
+--------+-------+--------------------------------+
| 2      | 0x02  | Message ID                     |
+--------+-------+--------------------------------+
| 3      | 0x00  | Payload Length                 |
+--------+-------+--------------------------------+
| 4      | 0x8D  | Payload CRC8                   |
+--------+-------+--------------------------------+
| 5      | 0x67  | Header CRC8                    |
+--------+-------+--------------------------------+
| 6      | 0x00  | Command Response Code          |
+--------+-------+--------------------------------+
| 7 - 12 |       | BT MAC: *\* 98 07 2D 05 FE 54* |
+--------+-------+--------------------------------+

Read Firmware Version
~~~~~~~~~~~~~~~~~~~~~

Send this command to retrieve the Firmware version of the serial beacon.

.. _command-3:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x03  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0xCD  | Header CRC8    |
+------+-------+----------------+

.. _response-3:

Response
~~~~~~~~

*Example Response:* **00 BC 03 03 48 0D 00 02 16**

+-------+-------+-----------------------+
| Byte  | Value | Description           |
+=======+=======+=======================+
| 0     | 0x00  | Message type          |
+-------+-------+-----------------------+
| 1     | 0xBC  | Class ID              |
+-------+-------+-----------------------+
| 2     | 0x03  | Message ID            |
+-------+-------+-----------------------+
| 3     | 0x03  | Payload Length        |
+-------+-------+-----------------------+
| 4     | 0x48  | Payload CRC8          |
+-------+-------+-----------------------+
| 5     | 0x0D  | Header CRC8           |
+-------+-------+-----------------------+
| 6     | 0x00  | Command Response Code |
+-------+-------+-----------------------+
| 7 - 8 |       | Firmware version      |
+-------+-------+-----------------------+

Read Firmware UID
~~~~~~~~~~~~~~~~~

Send this command to retrieve the Firmware UID of the serial beacon.

.. _command-4:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x04  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0xDB  | Header CRC8    |
+------+-------+----------------+

.. _response-4:

Response
~~~~~~~~

*Example Response:* **00 BC 04 05 81 14 00 00 11 02 16**

+--------+-------+-----------------------+
| Byte   | Value | Description           |
+========+=======+=======================+
| 0      | 0x00  | Message type          |
+--------+-------+-----------------------+
| 1      | 0xBC  | Class ID              |
+--------+-------+-----------------------+
| 2      | 0x04  | Message ID            |
+--------+-------+-----------------------+
| 3      | 0x05  | Payload Length        |
+--------+-------+-----------------------+
| 4      | 0x81  | Payload CRC8          |
+--------+-------+-----------------------+
| 5      | 0x14  | Header CRC8           |
+--------+-------+-----------------------+
| 6      | 0x00  | Command Response Code |
+--------+-------+-----------------------+
| 7 - 10 |       | Firmware UID          |
+--------+-------+-----------------------+

Read Model Number
~~~~~~~~~~~~~~~~~

Send this command to retrieve the Model Number of the serial beacon.

.. _command-5:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x05  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0xB0  | Header CRC8    |
+------+-------+----------------+

.. _response-5:

Response
~~~~~~~~

*Example Response:* **00 BC 05 03 77 CD 00 00 11**

+-------+-------+-------------------------+
| Byte  | Value | Description             |
+=======+=======+=========================+
| 0     | 0x00  | Message type            |
+-------+-------+-------------------------+
| 1     | 0xBC  | Class ID                |
+-------+-------+-------------------------+
| 2     | 0x05  | Message ID              |
+-------+-------+-------------------------+
| 3     | 0x03  | Payload Length          |
+-------+-------+-------------------------+
| 4     | 0x77  | Payload CRC8            |
+-------+-------+-------------------------+
| 5     | 0xCD  | Header CRC8             |
+-------+-------+-------------------------+
| 6     | 0x00  | Command Response Code   |
+-------+-------+-------------------------+
| 7 - 8 |       | Model Number\ *\*00 11* |
+-------+-------+-------------------------+

Read Status
~~~~~~~~~~~

Send this command to retrieve the current status of the serial beacon.

.. _command-6:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x06  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0x0D  | Header CRC8    |
+------+-------+----------------+

.. _response-6:

Response
~~~~~~~~

*Example Response:* **00 BC 06 11 51 FF 00 98 07 2D 05 FE 54 00 11 02 16
00 00 00 BC BC BC**

+---------+----------------+--------------------------------------------------+
| Byte    | Value          | Description                                      |
+=========+================+==================================================+
| 0       | 0x00           | Message type                                     |
+---------+----------------+--------------------------------------------------+
| 1       | 0xBC           | Class ID                                         |
+---------+----------------+--------------------------------------------------+
| 2       | 0x06           | Message ID                                       |
+---------+----------------+--------------------------------------------------+
| 3       | 0x11           | Payload Length                                   |
+---------+----------------+--------------------------------------------------+
| 4       | 0x51           | Payload CRC8                                     |
+---------+----------------+--------------------------------------------------+
| 5       | 0xFF           | Header CRC8                                      |
+---------+----------------+--------------------------------------------------+
| 6       | 0x00           | Command Response Code                            |
+---------+----------------+--------------------------------------------------+
| 7-12    |                | Module BT MAC\ *\*98 07 2D 05 FE 54*             |
+---------+----------------+--------------------------------------------------+
| 13-16   |                | Firmware build\ *\*00 11 02 16*                  |
+---------+----------------+--------------------------------------------------+
| 17      |                | Settings Version\ *\*00*                         |
+---------+----------------+--------------------------------------------------+
| 18      |                | Measured Power at 1m\ *Stored as 2’s compliment* |
+---------+----------------+--------------------------------------------------+
| 19      |                | Battery Voltage\ **0 if not battery powered**    |
+---------+----------------+--------------------------------------------------+
| 20 - 22 | 0xBC 0xBC 0xBC | Fixed content                                    |
+---------+----------------+--------------------------------------------------+

Read Encrypted Status
~~~~~~~~~~~~~~~~~~~~~

Send this command to retrieve the current status of the serial beacon
but encrypted.

.. _command-7:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x07  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0x66  | Header CRC8    |
+------+-------+----------------+

.. _response-7:

Response
~~~~~~~~

*Example Response:* **00 BC 07 11 F6 E8 00 E5 9C 67 DF 1B A0 5D E0 23 33
23 A5 1D 4B 7D F8**

+------+-------+-----------------------+
| Byte | Value | Description           |
+======+=======+=======================+
| 0    | 0x00  | Message type          |
+------+-------+-----------------------+
| 1    | 0xBC  | Class ID              |
+------+-------+-----------------------+
| 2    | 0x06  | Message ID            |
+------+-------+-----------------------+
| 3    | 0x11  | Payload Length        |
+------+-------+-----------------------+
| 4    | 0xF6  | Payload CRC8          |
+------+-------+-----------------------+
| 5    | 0xE8  | Header CRC8           |
+------+-------+-----------------------+
| 6    | 0x00  | Command Response Code |
+------+-------+-----------------------+
| 7-22 |       | Encrypted Data        |
+------+-------+-----------------------+

Start Scanning
~~~~~~~~~~~~~~

This command returns a live stream of detected BLE devices ad packets.
We recommend starting and stopping on intervals (for instance 1 second).

Start Scanning is one of the commands that is sent with a payload.

The payload consists of:

+--------------------+-----------------+-------------------------------+
| Byte               | Allowed Values  | Description                   |
+====================+=================+===============================+
| 0                  | 0x00 - 0x03     | Discovery Mode                |
+--------------------+-----------------+-------------------------------+
| 1                  | 0x00 - 0xFF     | Number of results per Scan    |
|                    |                 | Duration                      |
+--------------------+-----------------+-------------------------------+
| 2-3                | 0x0000 - 0xFFFF | Scan Duration in ms, >= Scan  |
|                    |                 | Interval\ **Note little       |
|                    |                 | endian**                      |
+--------------------+-----------------+-------------------------------+
| 4-5                | 0x0000 - 0xFFFF | Scan Interval in units of     |
|                    |                 | 0.625 ms, >= Scan Window      |
|                    |                 | \ **Note little endian**      |
+--------------------+-----------------+-------------------------------+
| 6-7                | 0x00 - 0xFF     | Scan Window in units of 0.625 |
|                    |                 | ms, scan active \ **Note      |
|                    |                 | little endian**               |
+--------------------+-----------------+-------------------------------+
| 8                  | 0x00 - 0xFF     | Discover Active Scan mode     |
+--------------------+-----------------+-------------------------------+

For full descriptions on all of these variables, please visit the Texas
Instruments website

.. raw:: html

   <!-- ###(link to follow) -->

**Example command:** **00 BC 08 09 DF 8F 03 05 64 00 35 00 35 00 00**

.. _command-8:

Command
~~~~~~~

+-------+--------+-----------------------------+
| Byte  | Value  | Description                 |
+=======+========+=============================+
| 0     | 0x00   | Message type                |
+-------+--------+-----------------------------+
| 1     | 0xBC   | Class ID                    |
+-------+--------+-----------------------------+
| 2     | 0x08   | Command ID                  |
+-------+--------+-----------------------------+
| 3     | 0x09   | Payload Length              |
+-------+--------+-----------------------------+
| 4     | 0xDF   | Payload CRC8                |
+-------+--------+-----------------------------+
| 5     | 0x8F   | Header CRC8                 |
+-------+--------+-----------------------------+
| 6     | 0x03   | Mode 3                      |
+-------+--------+-----------------------------+
| 7     | 0x05   | 5 Results per Scan Duration |
+-------+--------+-----------------------------+
| 8-9   | 0x6400 | 100 ms                      |
+-------+--------+-----------------------------+
| 10-11 | 0x3500 | 53 \* 0.625 = 33.125 ms     |
+-------+--------+-----------------------------+
| 12-13 | 0x3500 | 53 \* 0.625 = 33.125 ms     |
+-------+--------+-----------------------------+
| 14    | 0x00   | Discover Active Scan Mode 0 |
+-------+--------+-----------------------------+

.. _response-8:

Response
~~~~~~~~

*Example Response:* **00 BC 08 01 00 34 00 80 BC 0B 00 00 0B 80 BC 10 12
14 54 7D 42 99 FB 51 61 CC 02 01 1A 07 FF 4C 00 10 02 0A 00 80 BC 10 25
20 4A 47 01 00 02 22 D4 BB 02 01 06 1A FF 4C 00 02 15 E2 C5 6D B5 DF FB
48 D2 B0 60 D0 F5 A7 10 96 E0 00 00 00 00 C5 80 BC 10 12 DE 2C 7D 80 00
50 E6 80 CF 02 01 06 07 FF 4C 00 10 02 0B 00 80 BC 10 0E 39 3C 84 F3 07
89 71 24 BC 02 01 06 03 02 F0 FF 80 BC ………..**

We can see in the above example, a standard Response PDU (00 BC 08 01 00
34 00), followed by a lot of Bluecats Event PDUs (80 BC 0B …..) or other
BLE PDUs (for instance iBeacon or Eddystone).

Note that each Event PDU starts with 80 BC which means [Event] [Class
ID]. The first Event PDU shows that scanning has started as the Event ID
(0B). `Full list of event IDs <#eventResponses>`__

To decipher all the data coming through, you will need to make sure you
use the length variable to parse the payloads and recognise each packet
type.

**Two most important PDUs from the very start of each start scanning
command follow:**

+------+-------+--------------------------------------------------+
| Byte | Value | Description                                      |
+======+=======+==================================================+
| 0    | 0x00  | Message type                                     |
+------+-------+--------------------------------------------------+
| 1    | 0xBC  | Class ID                                         |
+------+-------+--------------------------------------------------+
| 2    | 0x08  | Message ID                                       |
+------+-------+--------------------------------------------------+
| 3    | 0x01  | Payload Length                                   |
+------+-------+--------------------------------------------------+
| 4    | 0x00  | Payload CRC8                                     |
+------+-------+--------------------------------------------------+
| 5    | 0x34  | Header CRC8                                      |
+------+-------+--------------------------------------------------+
| 6    | 0x00  | Command Response Code                            |
+------+-------+--------------------------------------------------+
| 7    | 0x80  | Message Type 0x80 (event)                        |
+------+-------+--------------------------------------------------+
| 8    | 0xBC  | Class ID                                         |
+------+-------+--------------------------------------------------+
| 9    | 0x0B  | BLE Scanning Stopped                             |
+------+-------+--------------------------------------------------+
| 10   | 0x00  | Payload Length                                   |
+------+-------+--------------------------------------------------+
| 11   | 0x00  | Payload CRC8                                     |
+------+-------+--------------------------------------------------+
| 12   | 0x1D  | Header CRC8                                      |
+------+-------+--------------------------------------------------+
| 13-n |       | Blocks of Events [0x0D - 0x10] (#eventResponses) |
+------+-------+--------------------------------------------------+

Stop Scanning
~~~~~~~~~~~~~

Stop scanning - this will work whether scanning is running or not.

This command will return a response and an event.

.. _command-9:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x09  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0x4A  | Header CRC8    |
+------+-------+----------------+

.. _response-9:

Response
~~~~~~~~

*Example Response:* **00 BC 09 01 00 5F 00 80 BC 0C 00 00 1D**

+------+-------+---------------------------+
| Byte | Value | Description               |
+======+=======+===========================+
| 0    | 0x00  | Message type              |
+------+-------+---------------------------+
| 1    | 0xBC  | Class ID                  |
+------+-------+---------------------------+
| 2    | 0x09  | Message ID                |
+------+-------+---------------------------+
| 3    | 0x01  | Payload Length            |
+------+-------+---------------------------+
| 4    | 0x00  | Payload CRC8              |
+------+-------+---------------------------+
| 5    | 0x5F  | Header CRC8               |
+------+-------+---------------------------+
| 6    | 0x00  | Command Response Code     |
+------+-------+---------------------------+
| 7    | 0x80  | Message Type 0x80 (event) |
+------+-------+---------------------------+
| 8    | 0xBC  | Class ID                  |
+------+-------+---------------------------+
| 9    | 0x0C  | BLE Scanning Stopped      |
+------+-------+---------------------------+
| 10   | 0x00  | Payload Length            |
+------+-------+---------------------------+
| 11   | 0x00  | Payload CRC8              |
+------+-------+---------------------------+
| 12   | 0x1D  | Header CRC8               |
+------+-------+---------------------------+

Go DFU mode
~~~~~~~~~~~

Enter Device Firmware Update mode.

This command will return a response and an event.

.. _command-10:

Command
~~~~~~~

+------+-------+----------------+
| Byte | Value | Description    |
+======+=======+================+
| 0    | 0x00  | Message type   |
+------+-------+----------------+
| 1    | 0xBC  | Class ID       |
+------+-------+----------------+
| 2    | 0x0C  | Command ID     |
+------+-------+----------------+
| 3    | 0x00  | Payload Length |
+------+-------+----------------+
| 4    | 0x00  | Payload CRC8   |
+------+-------+----------------+
| 5    | 0x8A  | Header CRC8    |
+------+-------+----------------+

.. _response-10:

Response
~~~~~~~~

*Example Response:* **00 BC 09 01 15 F4 07** > **Not
supported**\ Payload too short

+------+-------+-----------------------+
| Byte | Value | Description           |
+======+=======+=======================+
| 0    | 0x00  | Message type          |
+------+-------+-----------------------+
| 1    | 0xBC  | Class ID              |
+------+-------+-----------------------+
| 2    | 0x09  | Message ID            |
+------+-------+-----------------------+
| 3    | 0x01  | Payload Length        |
+------+-------+-----------------------+
| 4    | 0x15  | Payload CRC8          |
+------+-------+-----------------------+
| 5    | 0xF4  | Header CRC8           |
+------+-------+-----------------------+
| 6    | 0x07  | Command Response Code |
+------+-------+-----------------------+

..

   Remainder incomplete stop reading from this point forward

::

   #define SERAPI_CMD_WRITE_FW_HDR                    0x0A
   #define SERAPI_CMD_WRITE_FW_BLK                    0x0B
