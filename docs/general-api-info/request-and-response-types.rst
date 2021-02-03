.. _request-and-response:

==========================
Request and response types
==========================

The |apiservice| supports JSON data serialization formats. You can specify the
request format by using the ``Content-Type`` header, and operations that have a
request body must have this header. You can specify the response format in
requests by using the ``Accept`` header. Note that JSON is the default and only 
format for data serialization.

**Response formats**

+--------+-------------------------+---------+
| Format |   Content-Type header   | Default |
+========+=========================+=========+
| JSON   |    application/json     |   Yes   |
+--------+-------------------------+---------+
