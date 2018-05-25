BlueCats beacons have a special ability to transfer data to and from
your app and optionally a host (for instance if you are using our Atmo
or USB beacons).

This is particularly useful if you would like to perform some sort of
transaction, receive data from a host computer, or transfer data between
two phones using a beacon as an intermediary.

Requesting Data from a *hosted* Beacon
--------------------------------------

Requesting data from a beacon can be done with the BCLassoManager. A
BCLasso represents a hosted BlueCats beacon. This beacon can be an
embedded BlueCats module or a BlueCats USB beacon connected to a host.

Request data from a beacon endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Java
''''

.. code:: java

   final List<ByteBuffer> requestData = new ArrayList<>();
   requestData.add( ByteBuffer.wrap( ( "request data" ).getBytes() ) );

   beacon.requestDataArrayFromBeaconEndpoint( BCBeaconUpdates.BCBeaconEndpoint.BC_BEACON_ENDPOINT_USB_HOST, requestData, new BCBeaconCommandCallback()
       {
           @Override
           public void onDidComplete( final BCError error )
           {
               //Operation complete
           }

           @Override
           public void onDidUpdateProgress( final int percent, final String status )
           {
               //How far through the operation we are
           }

           @Override
           public void onDidUpdateStatus()
           {
               //Status update
           }

           @Override
           public void onDidResponseData( final List<ByteBuffer> responseData )
           {
               //Success
           }
       }
   );

Objective-C
'''''''''''

.. code:: objc

   - (void)requestSomeData:(BCBeacon *)beacon requestData:(NSData *)requestData {
       NSString* requestDataString = [[NSString alloc] initWithData:requestData encoding: NSUTF8StringEncoding];
       NSArray* requestDataArray = [NSArray arrayWithObject:requestDataString];

       [beacon requestDataArrayFromBeaconEndpoint:BCBeaconEndpointUSBHost withDataArray:requestDataArray success:^(NSArray* responseDataArray) {
           //success
       } status:^(NSString* status) {
           //status
       } failure:^(NSError* error) {
           //error
       }];
   }

Swift
'''''

.. code:: swift

   func requestSomeData(beacon: BCBeacon) {
       let requestData: [AnyObject] = ["request data"]
       beacon.requestDataArrayFromBeaconEndpoint(BCBeaconEndpoint.USBHost, withDataArray: requestData, success: { (responseDataArray) -> Void in
               //success
           }, status: { (status) -> Void in
               //status
           }) { (error) -> Void in
               print(error)
       }
   }

**More To Come** **Need to cover off how to transfer data to the host,
or to another phone (IE. transferring a URL pointing to a cloud hosted
file).**
