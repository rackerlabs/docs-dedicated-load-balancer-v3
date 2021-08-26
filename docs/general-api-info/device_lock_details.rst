.. _device_lock_details:

====================================================
Enhancements in V3 for multiple requests on a device
====================================================

The mechanism for executing multiple requests in the Load Balancer v3 API
is improved.

To illustrate this, the following sections compare the v2 and v3 API mechanism
through an example:

Enqueued process mechanism in v2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Device `123456` exists under account `1111`.
2. When a user executes a POST or PUT request, it returns the status
   `200 OK`, with the following response body:

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


3. If the user re-executes the request before the first one completes, the
   request goes into `ENQUEUED_PROCESSING`:


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

4. The process might get dead-locked, so the user keeps re-executing the
   request thinking they're not running. All of these
   requests get queued on the server awaiting execution.

The user has no idea why the requests never finish, so v3 has a lock mechanism
on the device to avoid this issue.

Lock mechanism in v3
^^^^^^^^^^^^^^^^^^^^

1. Device `123456` exists under account `1111`.

2. When the user executes a POST or PUT request, the returned status is
   `200 OK`, with the following response body:

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

3. If the user executes another the request before the first one completes,
   the returned response code is `423 Locked` with the
   following repose body:

::

    {
        "request_url": "{{request_url_str}}",
        "details": "Another event already scheduled to launch on Device 535908",
        "statusCode": 423,
        "title": "423 Locked",
        "transactionId": "{{trans_id_str}}"
    }

4. This process avoids any chance of deadlock, and as soon as the request gets
   completed, the device goes out of lock.

This mechanism lets the user know what is happening on the back-end. Even if a
request doesn't complete, the system removes the
device from lock after a set length of time to avoid inconvenience for the
user.
