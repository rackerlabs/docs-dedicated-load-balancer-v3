.. _device_lock_details:

===================================================
Enhancements in V3 for multiple request on a device
===================================================

The mechanism for hitting multiple requests has been changed in V3 Load Balancer API.

To understand this let's understand what used to be the mechanism in API 2.0 through an example :

Enqueued Process Mechanism in v2.0
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. We have a device 123456 under account 1111.
2. While hitting a POST or PUT request the returned status is 200 OK. With response body as under :

::

    {
        "data": {
            "status": "PROCESSING",
            "eventId": "{{event_id}}",
            "resource": "POOL-172.16.1.106-80",
            "timestamp": "2021-01-14T06:58:17.7277401Z",
            "eventRef": "/events/{{event_id}}"
         }
    }


3. If the user hits the same request before the first one gets completed. The request goes into "ENQUEUED_PROCESSING"


::

    {
        "data": [
            {
                "event_id": "{{event_id_str}}",
                "status": 200,
                "message": "ENQUEUED_PREPROCESSING",
                "entrytimestamp": "2021-01-14T06:35:35",
                "modifiedtimestamp": "2021-01-14T06:35:35"
            }
        ]
    }

4. There is a chance that the process goes into dead-lock and the user keeps on hitting the request thinking it's not getting completed. All of these request get queued into the server and waiting for them to complete.

5. The user has no idea as to why his request isn't getting completed.

To avoid this scenario. We have implemented a lock mechanism on the device.

Lock Mechanism in V3.0
^^^^^^^^^^^^^^^^^^^^^^^

In API V3.0 the scenario is as under :

1.  We have a device 123456 under account 1111.

2.  While hitting a POST or PUT request the returned status is 200 OK. With response body as under :

::

    {
        "data": {
            "eventId": "{{event_id_str}}",
            "status": "PROCESSING",
            "timestamp": "2021-01-14T07:11:49.885096Z",
            "resource": "POOL-172.16.1.109-80",
            "eventRef": "/events/{{event_id_str}}"
        }
    }

3. When the user hits another request before the first one gets completed. The returned response code is 423 Locked and body is as under :

::

    {
        "request_url": "{{request_url_str}}",
        "details": "Another event already scheduled to launch on Device 535908",
        "statusCode": 423,
        "title": "423 Locked",
        "transactionId": "{{trans_id_str}}"
    }

4. This avoids any chance of deadlock and as soon as the request gets completed the device goes out of lock.

5. This mechanism brings a clarity to the user as to what is happening at the back-end.

6. Even when a request doesn't get completed the device is removed from lock for user to access after a certain set amount of time to avoid inconvenience for the user.