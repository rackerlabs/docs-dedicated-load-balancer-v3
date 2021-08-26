.. _general-api-info:

=======================
General API Information
=======================

The |product name| API is implemented using a RESTful web service interface.
|product name| shares a common token-based authentication system that enables
seamless access across Rackspace products and services.

The following information is relevant to all operations of the API. For
details about specific operations, see the API reference sections.

.. toctree::
   :maxdepth: 1

   service-access-endpoints.rst
   api-contract-version.rst
   request-and-response-types.rst
   date-time-format.rst
   faults.rst
   limits.rst
   device_lock_details.rst


.. note::
   All requests to authenticate against and operate the service are
   performed using SSL over HTTP (HTTPS) on TCP port `443`. For authentication
   instructions, see
   :ref:`Authenticate to the Rackspace Cloud <authenticate-to-identity-service>`.
