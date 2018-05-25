Viewing local logs
~~~~~~~~~~~~~~~~~~

The local device logs can be used to see if beacons are being detected
and if they are being successfully sent to the configured endpoint. For
example the following logs show that the UDP endpoint is not available:

.. raw:: html

   <p align="center">

.. raw:: html

   </p>

Resetting Edge To Default Firmware When Locked Out
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Connect Edge LAN into your computer with ethernet cable
2. Unplug Edge power
3. Hold Reset
4. Plug in power while holding reset. Edge will flash some LEDs, and
   then start blinking red LED. Let red LED blink 6 times, then let go
   of reset. You should see the red LED blink fast for 1 sec
5. Disable your WiFi, and set your own ip to 192.168.1.2 for your
   ethernet interface

   .. raw:: html

      <p align="center">

   .. raw:: html

      </p>

6. Go to 192.168.1.1 in your web browser
7. Download our firmware > ## Downloading the New Firmware >\* Go to our
   BlueCats App and login with your BlueCats account
   https://app.bluecats.com/login >\* On the left hand side, click on
   the “Devices” tab >\* Find your Edge and click on the “Settings” or
   the gear icon in the upper right corner >\* Click and download the
   latest Edge and BLE firmwares

   .. raw:: html

      <p align="center">

   .. raw:: html

      </p>

8. Upload and apply the firmware

.. raw:: html

   <p align="center">

.. raw:: html

   </p>

9. Wait a minute. Set your IP to something like 192.168.8.118 (just not
   192.168.8.1) for the Ethernet interface and go to 192.168.8.1 in your
   browser. You should see the Bluecats OpenWrt web interface.
