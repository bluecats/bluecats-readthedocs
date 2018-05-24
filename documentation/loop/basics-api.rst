This describes the resources for the Loop API. If you have any problems
or requests contact support@bluecats.com.

   .. rubric:: [Loop API 0.3.3]({{
      ‘documentation/loop/loop-versions/loop-api-0.3.3’ \| relative_url
      }})
      :name: loop-api-0.3.3-documentationlooploop-versionsloop-api-0.3.3-relative_url

..

      -  Group By Datepart - functions are now allowed in group by
         selections, for now limited to ‘year’, ‘month’, ‘day’, and
         ‘hour’ on datetime fields, for instance the following query.
      -  GET events now supports the ‘tsStart’ and ‘tsEnd’ url
         parameters, these optional parameters must be included together
         if used, should be in the standard datetime string format we’ve
         been using, and will act as ‘between’ selectors (inclusive) for
         the returned events.
      -  POST objects (object creation) will now accept a 32 character
         hex string in the ‘id’ field which will then be used as the
         object’s id.

   .. rubric:: [Loop API 0.3.2]({{
      ‘documentation/loop/loop-versions/loop-api-0.3.2’ \| relative_url
      }})
      :name: loop-api-0.3.2-documentationlooploop-versionsloop-api-0.3.2-relative_url

      -  Schema field descriptions - Basically a list of all the keys
         that could be in a schema object and what they mean.
      -  Search now supports IN for where clauses. Example Below:

..

   .. rubric:: [Loop API 0.3.1]({{
      ‘documentation/loop/loop-versions/loop-api-0.3.1’ \| relative_url
      }})
      :name: loop-api-0.3.1-documentationlooploop-versionsloop-api-0.3.1-relative_url

      -  For Event Queries, ObjectID is now required and limit is now
         optional.
      -  Object create and edit response has changed to return json
         dictionary with objectType and id e.g. {“objectType”: “Device”,
         “id”: “4b3890e4fb580c6e9443b6081d0a1628”}.
      -  Search: from can include log type.
      -  Add header X-API-HEADER to EVERY request, value of 1.
      -  Groups now have placeParentID in schema.

..

   .. rubric:: [Loop API 0.3.0]({{
      ‘documentation/loop/loop-versions/loop-api-0.3.0’ \| relative_url
      }})
      :name: loop-api-0.3.0-documentationlooploop-versionsloop-api-0.3.0-relative_url

      -  Login is now a POST to receive credentials instead of GET.
      -  GET The new Schema endpoint allows to search more information
         about what is avialble for Objects.
      -  PATCH or DELETE to the objects endpoint allows for editing and
         deleting objects is now availible.
      -  POST JSON to the Search Endpoint which allows search resources
         with Clause dictionaries and Comparison Operators.

..

   .. rubric:: [Loop API 0.2.1]({{
      ‘documentation/loop/loop-versions/loop-api-0.2.1’ \| relative_url
      }})
      :name: loop-api-0.2.1-documentationlooploop-versionsloop-api-0.2.1-relative_url

      -  Loop API 0.2.1 is the first release of the Loop API, which
         allows for Object creation, Event creation, object search, and
         much more.
