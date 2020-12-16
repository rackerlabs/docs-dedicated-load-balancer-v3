Retrieve device information
---------------------------

Use the device ID operation to get complete information about the device
with the specified ID including associated customer, usage statistics,
and configuration details for nodes, virtual IPs, and high availability.

::

    GET /{device_id}

*This operation does not accept a request body.*

Response
^^^^^^^^^

::

    {
    "load_balancer_data": {
        "id": "222222",
        "customer": "222222",
        "hostname": "adx1000-skynet.iad3.netdev.net",
        "management_ip": "22.22.22.222",
        "model_name": "SI-1216-4-PREM",
        "os_version": "12.5.02gT403",
        "role": "unimplemented",
        "uptime": "unimplemented",
        "cpu_load": [
            {
                "mod_name": "MASTER_CPU",
                "average": {
                    "percent_load": 1,
                    "seconds_since": 7256
                },
                "last_peak_load": {
                    "percent_load": 491,
                    "seconds_since": 1
                },
                "load_1_sec_avg": {
                    "percent_load": 491,
                    "seconds_since": 1
                },
                "load_5_sec_avg": {
                    "percent_load": 98,
                    "seconds_since": 5
                },
                "load_60_sec_avg": {
                    "percent_load": 9,
                    "seconds_since": 60
                },
                "load_300_sec_avg": {
                    "percent_load": 9,
                    "seconds_since": 300
                }
            },
            {
                "mod_name": "ASB1/1",
                "average": {
                    "percent_load": -1,
                    "seconds_since": -1
                },
                "last_peak_load": {
                    "percent_load": 1,
                    "seconds_since": 4
                },
                "load_1_sec_avg": {
                    "percent_load": 1,
                    "seconds_since": 1
                },
                "load_5_sec_avg": {
                    "percent_load": 1,
                    "seconds_since": 5
                },
                "load_60_sec_avg": {
                    "percent_load": 1,
                    "seconds_since": 60
                },
                "load_300_sec_avg": {
                    "percent_load": 1,
                    "seconds_since": 300
                }
            }
        ],
        "ram_mem": [
            {
                "name": "MP",
                "total_kbytes": "2097152",
                "used_kbytes": "1058236",
                "free_kbytes": "1038916"
            },
            {
                "name": "BP1",
                "total_kbytes": "456964",
                "used_kbytes": "69504",
                "free_kbytes": "387460"
            },
            {
                "name": "BP2",
                "total_kbytes": "456964",
                "used_kbytes": "69304",
                "free_kbytes": "387660"
            },
            {
                "name": "BP3",
                "total_kbytes": "456964",
                "used_kbytes": "69372",
                "free_kbytes": "387592"
            }
        ],
        "ha_role": "none",
        "ha_status": "none"
        }
    }

Retrieve virtual IPs configuration
----------------------------------

Retrieve information about all virtual servers configured in the load balancer including configuration data and status information.

::

    GET /{device_id}/vips

*This operation does not accept a request body.*

GET 200 response
^^^^^^^^^^^^^^^^

Successfully processed the request.

::

    {
    "load_balancer_data":[
        {
            "id": "VIP-146.20.75.33:22.22.22.222:78",
            "ip": "22.22.22.222",
            "label": "VIP-146.20.75.33",
            "description": "",
            "algorithm": {
                "name": "LEAST_CONNECTION",
                "persistence": null
            },
            "port_name": "78",
            "port_number": 78,
            "protocol": "TCP",
            "admin_state": "DISABLED",
            "runtime_state": "UNHEALTHY",
            "vendor_extensions": {
                "none": "none"
            },
            "stats": {
                "bytes_in": -1,
                "bytes_out": -1,
                "conn_cur": 0,
                "conn_max": -1,
                "conn_tot": 0,
                "pkts_in": 0,
                "pkts_out": -1
            },
            "nodes": [
                {
                    "id": "label_DCXQE:5.6.7.2:8560",
                    "label": "label_DCXQE",
                    "ip": "5.6.7.2",
                    "port_name": "8560",
                    "port_number": 8560
                }
            ]
        },
        {
            "id": "Vip-Test-68f31107:22.22.22.222:80",
            "ip": "22.22.22.222",
            "label": "Vip-Test-68f31107",
            "description": "",
            "algorithm": {
                "name": "LEAST_CONNECTION",
                "persistence": null
            },
            "port_name": "HTTP",
            "port_number": 80,
            "protocol": "TCP",
            "admin_state": "ENABLED",
            "runtime_state": "UNHEALTHY",
            "vendor_extensions": {
                "none": "none"
            },
            "stats": {
                "bytes_in": -1,
                "bytes_out": -1,
                "conn_cur": 0,
                "conn_max": -1,
                "conn_tot": 0,
                "pkts_in": 0,
                "pkts_out": -1
            },
            "nodes": [
                {
                    "id": "Node-Test-68f31107:22.22.22.222:80",
                    "label": "Node-Test-68f31107",
                    "ip": "22.22.22.222",
                    "port_name": "HTTP",
                    "port_number": 80
                },
                {
                    "id": "Node-Test-Id:22.22.22.222:80",
                    "label": "Node-Test-Id",
                    "ip": "22.22.22.222",
                    "port_name": "HTTP",
                    "port_number": 80
                }
            ]
        }
        ]
    }


Retrieve event information by event ID.
---------------------------------------

Retrieve event information by using the event ID.

::

    GET /{device_id}/events/{event_id}

*This operation does not accept a request body.*

202 Response
^^^^^^^^^^^^

Successfully processed the request.

::

    {
        "events": [
            {
                "event_id": "<event-id(str)>",
                "message": "COMPLETED",
                "entrytimestamp": "2020-11-02T08:02:46.291000",
                "modifiedtimestamp": "2020-11-02T08:02:47.643000",
                "status": 200,
                "@type": "Event"
            }
        ]
    }


Add a Virtual IP
----------------

Add a virtual server configuration to the load balancer.

::

    POST /{device_id}/vips


Request
^^^^^^^

The ``account number``, ``port_name``, ``description``, and ``comment`` parameters are optional.

You can find ``persistence`` in the ``algorithm`` section, and it is an `enabled` or `disabled` field.

``port`` is an alias for ``port_number``.

::

    {
      "account_number": "<Account Number>",
      "label": req"<Label>",
      "description": "<description>",
      "ip": "<ip>",
      "protocol": req"<protocol>",
      "port": req"<port>",
      "algorithm": req{
                        "name": "<name>",
                        "persistence":req"<enabled|disabled>"
                       },
      "nodes": [],
      "admin_state": req"<enabled|disabled>",
      "comment": "comment"
    }

Response
^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-10-30 10:23:37.091993",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "event-id(str)"
    }


Retrieve virtual IP information
-------------------------------

Use this operation to retrieve 
information for a virtual IP configured for the specified device ID.

If you don't know the ID for a specified virtual IP, use this operation to find it.

::

    GET /{device_id}/vips/{vip_id}

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

    {
        "load_balancer_data": {
            "stats": {
                "conn_max": -1,
                "pkts_out": -1,
                "bytes_in": -1,
                "pkts_in": 0,
                "conn_tot": 0,
                "conn_cur": 0,
                "bytes_out": -1
            },
            "protocol": "TCP",
            "description": "Some description",
            "algorithm": {
                "name": "LEAST_CONNECTION",
                "persistence": "DISABLED",
                "persistence_method": "client_ip",
                "subnet_prefix_length": 0
            },
            "ip": "22.22.22.222",
            "runtime_state": "UNHEALTHY",
            "label": "Vip-Test-68f31107",
            "port_name": "HTTP",
            "admin_state": "ENABLED",
            "port_number": 80,
            "nodes": [
                {
                    "ip": "22.22.22.222",
                    "label": "Node-Test-68f31107",
                    "port_name": "HTTP",
                    "port_number": 80,
                    "id": "Node-Test-ID:222.222.22.2:80"
                },
                {
                    "ip": "22.22.22.222",
                    "label": "Node-Test-8cb22fcb",
                    "port_name": "HTTP",
                    "port_number": 80,
                    "id": "Node-Test-ID:222.222.22.2:80"
                }
            ],
            "id": "Vip-Test-68f31107:222.222.22.2:80",
            "vendor_extensions": {
                "none": "none"
            }
        }
    }

Update virtual IP information
-----------------------------

Use this operation to update
information for a virtual IP configured for the specified device ID.

If you don't know the ID for a specified virtual IP, use the **retrieve
virtual IPs** configuration operation to find it.


::

    PUT /{device_id}/vips/{vip_id}


Request body
^^^^^^^^^^^^

The ``account number``, ``port_name``, ``description``, and ``comment`` parameters are optional.

You can find ``persistence`` in the ``algorithm`` section, and it is an `enabled` or `disabled` field.

``port`` is an alias for ``port_number``.


::

    {
      "account_number": "<Account Number>",
      "label": req"<Label>",
      "description": "<description>",
      "ip": "<ip>",
      "protocol": "<protocol>",
      "port": "<port>",
      "algorithm": {
                        "name": "<name>",
                        "persistence":req"<enabled|disabled>"
                       },
      "nodes": [],
      "admin_state": "<enabled|disabled>",
      "comment": "comment"
    }

PUT Virtual IPs information 202 response
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-10-30 11:23:24.125076",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}}/loadbalancers/{device-id}}/events/{event-id(str)}",
        "id": "event-id(str)"
    }

Delete a virtual IP
-------------------

Use this operation to remove a virtual IP from the device
configuration.

If you don't know the ID for a specified virtual IP, use the **retrieve
virtual IPs** operation to find it.

.. note:: the request body is optional for the delete operation.

::

    DELETE /{device_id}/vips/{vip_id}

Request body (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^
::

    {
      "account_number": "<Account Number>",
      "comment": "<comment>"
    }

Response
^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-10-30 11:35:34.315166",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}}/events/{event-id(str)}",
        "id": "{event-id(str)}}"
    }

List nodes for the specified virtual IP
----------------------------------------

Retrieve information about the nodes associated with a specified virtual
IP.

::

    GET /{device_id}/vips/{vip_id}/nodes

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

        {
        "load_balancer_data": {
            "stats": {
                "conn_max": -1,
                "pkts_out": -1,
                "bytes_in": -1,
                "pkts_in": 0,
                "conn_tot": 0,
                "conn_cur": 0,
                "bytes_out": -1
            },
            "protocol": "TCP",
            "description": "Some description",
            "algorithm": {
                "name": "LEAST_CONNECTION",
                "persistence": "DISABLED"
            },
            "ip": "22.22.22.2",
            "runtime_state": "UNHEALTHY",
            "label": "Vip-Test-68f31107",
            "port_name": "HTTP",
            "admin_state": "ENABLED",
            "port_number": 80,
            "nodes": [
                {
                    "ip": "22.22.22.2",
                    "label": "Node-Test-68f31107",
                    "port_name": "HTTP",
                    "port_number": 80,
                    "id": "Node-Test-68f31107:22.22.22.2:80"
                },
                {
                    "ip": "22.22.22.2",
                    "label": "Node-Test-8cb22fcb",
                    "port_name": "HTTP",
                    "port_number": 80,
                    "id": "Node-Test-8cb22fcb:22.22.22.2:80"
                }
            ],
            "id": "Vip-Test-68f31107:22.22.22.2:80",
            "vendor_extensions": {
                "none": "none"
            }
        }
    }

Assign node to virtual IP
-------------------------

Use this operation to add a
specified node from the virtual IP configuration.

*When you assign a node to a virtual IP, the following field is required:
``account\_number``.*

::

    POST /{device_id}/vips/{vip_id}/nodes/{node_id}

Request body
^^^^^^^^^^^^
::

    {
      "node_id": "<Node Id>"
    }


OR
^^

Request body for backward compatibility
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

    {
      "account_number": "<Account Number>"
    }

Response
^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 07:58:55.506927",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Remove node from virtual IP configuration
-----------------------------------------

Use this operation to remove a
specified node from the virtual IP configuration.


::

    DELETE /{device_id}/vips/{vip_id}/nodes/{node_id}

Response
^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:03:23.942368",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Enable a virtual IP
-------------------

Use this operation to enable a
virtual IP configured for a specified device.

::

    POST /{device_id}/vips/{vip_id}/configuration

Request body
^^^^^^^^^^^^
::

  {
      "admin_state": "ENABLED"
  }

OR
^^

Request body
^^^^^^^^^^^^
::

  {
    "account_number": "<Account Number> (required)"
  }

Response
^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:05:09.683313",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Disable a virtual IP
--------------------

Use this operation to  disable a
virtual IP configured for a specified device. 

.. note:: When using this feature to set drain connections to a VIP, you must monitor the VIP stats for connection details. See `Show virtual IP statistics`_ for more information.

.. note:: The request body is optional for the disable operation.

::

    DELETE /{device_id}/vips/{vip_id}/configuration


Request body Optional
^^^^^^^^^^^^^^^^^^^^^
::

  {
    "account_number": "<Account Number> (required)"
  }

202 Response
^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:14:07.461543",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Show virtual IP statistics
--------------------------

Retrieves usage data for the specified virtual IP.

::

    GET /{device_id}/vips/{vip_id}/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

        {
        "id": "Vip-Test-68f31107:222.222.22.2:80",
        "ip": "222.222.22.2",
        "label": "Vip-Test-68f31107",
        "description": "Some description",
        "algorithm": {
            "name": "LEAST_CONNECTION",
            "persistence": "DISABLED"
        },
        "port_name": "HTTP",
        "port_number": 80,
        "protocol": "TCP",
        "admin_state": "ENABLED",
        "runtime_state": "UNHEALTHY",
        "vendor_extensions": {
            "none": "none"
        },
        "stats": {
            "bytes_in": -1,
            "bytes_out": -1,
            "conn_cur": 0,
            "conn_max": -1,
            "conn_tot": 0,
            "pkts_in": 0,
            "pkts_out": -1
        },
        "nodes": [
            {
                "id": "Node-Test-68f31107:222.222.22.2:80",
                "label": "Node-Test-68f31107",
                "ip": "222.222.22.2",
                "port_name": "HTTP",
                "port_number": 80
            }
        ]
    }


Show Nodes for the given device id
-----------------------------------

A node is a back-end device providing a service on a specified IP and
port.

Use this operation to get information about the nodes configured
for a specified device


::

    GET /{device_id}/nodes

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

    {
        "load_balancer_data": [
            {
                "id": "Node-Test-68f31107:222.222.22.2:80",
                "label": "Node-Test-68f31107",
                "ip": "222.222.22.2",
                "port_name": "HTTP",
                "port_number": 80,
                "admin_state": "ENABLED",
                "runtime_state": "HEALTHY",
                "stats": {
                    "bytes_in": 0,
                    "bytes_out": 0,
                    "conn_cur": 0,
                    "conn_max": 0,
                    "conn_tot": 0,
                    "pkts_in": 0,
                    "pkts_out": 0
                }
            },
            {
                "id": "Configuration-testNode:222.222.22.2:8560",
                "label": "Configuration-testNode",
                "ip": "222.222.22.2",
                "port_name": "8560",
                "port_number": 8560,
                "admin_state": "ENABLED",
                "runtime_state": "UNHEALTHY",
                "stats": {
                    "bytes_in": 0,
                    "bytes_out": 0,
                    "conn_cur": 0,
                    "conn_max": 0,
                    "conn_tot": 0,
                    "pkts_in": 0,
                    "pkts_out": 0
                }
            }
       ]
    }


Add a node to a device
----------------------

Use this operation to add a node to a specified device.

When adding a node to a device, the following fields are required:
``label``, ``ip``, ``port``, ``admin_state``,
``health_strategy``, and ``vendor_extensions``.

::

    POST /{device_id}/nodes

Request body
^^^^^^^^^^^^^

::

    {
      "account_number": "<Account Number> ",
      "label": "<Node Label> (required)",
      "description": "<description>",
      "ip": "<ip> (required)",
      "port": "<port> (required)",
      "admin_state": "<enabled|disabled> (required)",
      "health_strategy": "<health_strategy JSON Object> (required)",
      "vendor_extensions": "<vendor_extension JSON object> (required)",
      "comment": "comment"
    }

202 Response
^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:19:48.702127",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Retrieve node information
-------------------------

Use this operation to view a specified node.

::

    GET /{device_id}/nodes/{node_id}

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

       {
        "load_balancer_data": {
            "id": "<node_id_string>",
            "label": "RdcTesting",
            "ip": "22.22.2.22",
            "description": null,
            "port_name": "HTTP",
            "port_number": 80,
            "weight": -1,
            "protocol": "TCP",
            "health_strategy": {
                "strategy": "HTTP_RES_CODE",
                "port_number": 80,
                "ssl": false,
                "method": "GET",
                "path": "/index.html",
                "http_codes_ok": [
                    301
                ],
                "http_body_pattern": null
            },
            "limit": -1,
            "admin_state": "ENABLED",
            "runtime_state": "UNHEALTHY",
            "vendor_extensions": {
                "reassign_count": 0
            },
            "stats": {
                "bytes_in": 0,
                "bytes_out": 0,
                "conn_cur": 0,
                "conn_max": 0,
                "conn_tot": 0,
                "pkts_in": 0,
                "pkts_out": 0
            }
        }
    }


Update node information
-----------------------

Use this operation to update a specified node.

::

    PUT /{device_id}/nodes/{node_id}


Request body
^^^^^^^^^^^^

::

    {
      "account_number": "<Account Number>",
      "ip": "<ip>",
      "description": "<description>",
      "port": "<port>",
      "label": "<Node Label>",
      "health_strategy": {},
      "admin_state": "<enabled|disabled>"
      "vendor_extensions": {},
      "comment": "<comment>"
    }

202 Response
^^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:22:57.435683",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Delete node from the device configuration
-----------------------------------------

Use this operation to remove a specified node.

.. note:: The request body is optional for the delete operation.

::

    DELETE /{device_id}/nodes/{node_id}

Request body Optional
^^^^^^^^^^^^^^^^^^^^^

::

  {
    "account_number": "<Account Number> (required)"
  }

202 Response
^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-12-10 14:05:09.385509",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Enable a node
-------------

Use this operation to enable the specified node
included in the device configuration.


::

    POST /{device_id}/nodes/{node_id}/configuration

Request body
^^^^^^^^^^^^
::

  {
      "admin_state": "ENABLED"
  }

OR
^^

Request body for backward compatibilty
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

  {
    "account_number": "<Account Number> (required)"
  }

202 Response
^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:09:00.586497",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Disable a node
--------------

Use this operation to disable a specified node
included in the device configuration.


.. note:: When using this feature to set drain connections to a node (such as during a maintenance), you must monitor the
   node stats for connection details. See `Show node statistics`_ for more.
   
.. note:: The request body is optional for the disable operation.

::

    DELETE /{device_id}/nodes/{node_id}/configuration

Request body Optional
^^^^^^^^^^^^^^^^^^^^^

::

  {
    "account_number": "<Account Number> (required)"
  }




202 Response
^^^^^^^^^^^^^

The request has been accepted for processing.

::

    {
        "status": 202,
        "timestamp": "2020-11-02 08:29:48.221513",
        "@type": "Event",
        "message": "Processing",
        "@ref": "{tenant-id}/loadbalancers/{device-id}/events/{event-id(str)}",
        "id": "{event-id(str)}"
    }

Show node statistics
--------------------

Retrieves usage data for a specified node ID.

::

    GET /{device_id}/nodes/{node_id}/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

Successfully processed the request.

::

    {
        "load_balancer_data": {
            "id": "RdcTestNode6:22.22.2.22:75",
            "label": "RdcTestNode6",
            "ip": "10.14.15.12",
            "description": null,
            "port_name": "75",
            "port_number": 75,
            "weight": -1,
            "protocol": "BOTH",
            "health_strategy": {
                "strategy": "TCP_PORT",
                "port_number": 75
            },
            "limit": -1,
            "admin_state": "ENABLED",
            "runtime_state": "UNHEALTHY",
            "vendor_extensions": {
                "reassign_count": 0
            },
            "stats": {
                "bytes_in": 0,
                "bytes_out": 0,
                "conn_cur": 0,
                "conn_max": 0,
                "conn_tot": 0,
                "pkts_in": 0,
                "pkts_out": 0
            }
        }
    }



Depricated API
--------------

Show high availability template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    GET /{device_id}/ha

List events
^^^^^^^^^^^

::

    GET /{device_id}/events


Deprecated API
--------------

Show high availability template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    GET /{device_id}/ha

List events
^^^^^^^^^^^

::

    GET /{device_id}/events

