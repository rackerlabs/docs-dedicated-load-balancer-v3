.. _authenticate-using-curl:


Authenticate by using cURL
~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow these steps to authenticate to the Rackspace Cloud by
using cURL:

- :ref:`Send authentication request<send-auth-req-curl>`
- :ref:`Review the authentication response<review-auth-resp>`
- :ref:`Configure environment variables <configure-environment-variables>`

.. _send-auth-req-curl:

Send an authentication request
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From a command prompt, send a **POST tokens** request to the Rackspace Cloud
Identity service.  Include your API access username and
:ref:`API key<get-credentials>` as shown in the following example.
Replace ``$APIAccessUserName`` with your API access username and ``$apiKey``
with your API key.

.. include:: ../common-gs/samples/auth-req-curl.rst


.. _review-auth-resp:

Review the authentication response
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If your credentials are valid, the Identity service returns an authentication
response that includes the following information:

- an authentication token
- a service catalog with information about the services you can access.
- user information and role assignments

.. note::

   For detailed information about the authentication response, see the
   :rax-devdocs:`Annotated authentication request and\
   response<cloud-identity/v2/developer-guide/#document-authentication-info\
   /sample-auth-req-response>` in the Rackspace Cloud API documentation.

In the following example, the ellipsis (...)  represents other service
endpoints that are not shown. The values shown in this and other examples
vary because the information returned is specific to your account.

**Example: Authentication response**

.. include:: ../common-gs/samples/auth-resp-json.rst

If the request was successful, it returns the following values that you need to
include when you make service requests to the Rackspace product API:

**token ID**:

    The token ID value is required to confirm your identity each time you
    access the service. Include it in the ``X-Auth-Token`` header for each
    API request.

    The ``expires`` attribute indicates the date and time that the token will
    expire, unless it is revoked before the expiration. To get a new token,
    submit another authentication request. For more information, see
    :rax-devdocs:`Manage authentication tokens
    <cloud-identity/v2/getting-started/manage-auth-tokens>`.


**tenant ID**:

    The tenant ID provides your account number. For most Rackspace Cloud
    service APIs, the tenant ID is appended to the API endpoint in the service
    catalog automatically. For Rackspace Cloud services, the tenant ID has the
    same value as the tenant name.


**endpoint**:

    The API endpoint provides the URL that you use to access the API service.

If the request failed, review the response message and the following error
message descriptions to determine next steps:

- If you see the following error message, review the authentication request
  for syntax or coding errors. If you are using cURL, see
  :ref:`Using cURL <how-curl-commands-work>`.

  .. code::

     400 Invalid request body: unable to parse Auth data. Please review XML or
     JSON formatting

- If you see the following error message, verify the authentication credentials
  submitted in the authentication request. If necessary, contact your Rackspace
  Cloud Administrator or Rackspace Support to get valid credentials.

  .. code::

     401 Unable to authenticate user with credentials provided.

..  note::
       For additional information about authentication errors, see the
       :rax-dev:`Identity API Reference documentation
       <docs/cloud-identity/v2/api-reference/token-operations/>`.


.. _configure-environment-variables:

Configure environment variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To make it easier to include the token ID, endpoint, and other
values in API requests, use the``export`` command to create environment
variables that can be substituted for the
actual values. For example, you can create an ``ENDPOINT`` variable to
store the URL for accessing an API service. To reference the value in an API
request, prefix the variable name with a ``$``, for example ``$ENDPOINT``.

.. include:: ../common-gs/using-env-variables.rst

**Create environment variables**

#. In the ``token`` section of the authentication response, copy the token
   ``id`` and value from the token object.

      .. include:: ../common-gs/samples/auth-token-object.rst

#. Export the token ID to an environment variable
   that can be supplied in the `X-Auth-Token` header required in each
   API request.

   .. code::

       $ export TOKEN="token-id"

   Replace *token-id* with the authentication token ``id`` value returned
   in the authentication response.

#. Export the tenant ID to an environment variable that you can supply in
   requests that require you to specify a tenant ID.

   .. code::

      $ export TENANT="tenant-id"


#. In the ``service catalog`` section of the authentication response, copy the
   ``publicURL`` value for the |apiservice|, version, and region that you want
   to access.


#. Copy the URL and export it to an environment variable.

   .. code::

        $ export ENDPOINT="publicURL"


   Replace *publicURL* with the publicURL value listed in the service catalog.
