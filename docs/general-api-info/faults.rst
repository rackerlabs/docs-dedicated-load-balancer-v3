.. _faults:

======
Faults
======

When an error occurs, the |apiservice| returns a fault object that contains an
HTTP error response code denoting the error type. In the body of the
response, the system returns additional information about the fault.

The following table lists possible fault types with their associated error
codes and descriptions:

.. list-table::
   :widths: 20 20 30
   :header-rows: 1

   * - Fault type
     - Associated error code
     - Description
   * - Bad Request
     - 400
     - The user-provided request contained an error.
   * - Unauthorized
     - 401
     - The supplied token is not authorized to access the resources. The token
       is either expired or invalid.
   * - Forbidden
     - 403
     - The request is for something forbidden. Authorization cannot help.
   * - Not Found
     - 404
     - The back-end services did not find anything matching the request URI.
   * - Conflict
     - 409
     - The requested resource cannot currently be operated on.
   * - Internal Server Error
     - 500
     - The server encountered an unexpected condition that prevented it from
       fulfilling the request.
   * - Not implemented
     - 501
     - Retrieving a specific monitor is not implemented.
   * - Not extended
     - 510
     - An unexpected error occurred
   * - Locked
     - 423
     - The device is locked. For detailed info -> :ref:`device_lock_details`
