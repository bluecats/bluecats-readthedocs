The BlueCats SDK uses Core Data and in some cases this can cause
conflicts. If you listen for Core Data notifications, remember to pass
in your own context to avoid receiving BlueCats Core Data notifications.

Please see the Apple documentation on
`notifications <https://developer.apple.com/library/ios/documentation/Cocoa/Reference/CoreDataFramework/Classes/NSManagedObjectContext_Class/#//apple_ref/doc/uid/TP30001182-SW38>`__.

Objective-C
'''''''''''

.. code:: objectivec

   [[NSNotificationCenter defaultCenter] addObserver:self
                                            selector:@selector(contextDidSaveMainQueueContext:)
                                                name:NSManagedObjectContextDidSaveNotification
                                              object:[self "YOURCONTEXT"]];

Swift
'''''

.. code:: swift

   NSNotificationCenter.defaultCenter().addObserver(self, selector: "contextDidSaveMainQueueContext:", name: NSManagedObjectContextDidSaveNotification, object: self.YOURCONTEXT)
