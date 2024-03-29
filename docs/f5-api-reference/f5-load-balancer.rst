.. _f5-load-balancer:

==========================
F5 load balancer API calls
==========================



Important concepts
~~~~~~~~~~~~~~~~~~

.. contents::
     :depth: 1
     :local:

.. _disable-vs-offline:

Disable versus offline
----------------------

You can configure the system to disable some resources or force them offline.
Notice the key differences.

The difference between disabling a pool member or node or marking it offline
concerns the connections they still permit.

* **Disable**: Allows active and persistent connections.
* **Offline**: Allows only active connections.

These differences are usually significant when you perform connection draining
and require granularity over what sessions are still permitted.

Retrieve load balancer information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Retrieve load balancer information, such as the model number, OS version,
CPU statistics, and so on.

::

    GET /

*This operation does not accept a request body.*

**Response**

::

    {
      "data": [{
        "customer": "1234567",
        "chassis_serial": "f5-gvop-hkiv",
        "uptime": "396 days,  9:22",
        "ha_role": "true",
        "hostname": "1234567-lb1.example.rackspace.com",
        "firmware_version": "",
        "ram_mem": [{
          "total_kbytes": "4158218",
          "free_kbytes": "162846",
          "name": "TMM",
          "used_kbytes": "2860515"
        }],
        "model_name": "BIG-IP 1600",
        "os_version": "11.5.1, build: 3.0.131, edition: Hotfix HF3",
        "management_ip": "127.0.0.1",
        "role": "unimplemented",
        "cpu_load": [{
          "load_1_sec_avg": {
            "percent_load": 19,
            "seconds_since": 0
          },
          "average": {
            "percent_load": 17,
            "seconds_since": 0
          },
          "load_60_sec_avg": {
            "percent_load": -1,
            "seconds_since": -1
          },
          "last_peak_load": {
            "percent_load": 50,
            "seconds_since": 0
          },
          "mod_name": "System CPU",
          "load_5_sec_avg": {
            "percent_load": -1,
            "seconds_since": -1
          },
          "load_300_sec_avg": {
            "percent_load": -1,
            "seconds_since": -1
          }
        }],
        "ha_status": "active",
        "id": "1234567"
      }]
    }


Nodes
~~~~~

Nodes are a combination of an IP and a port that process requests
directed from a pool in a virtual server. You can bind nodes to one or more
pools.

Use the nodes API operations to list, delete, and update nodes.

.. contents::
     :depth: 1
     :local:

Retrieve nodes
--------------

Retrieve all nodes configured in the load balancer.


::

    GET /nodes

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve a list of nodes

::

    {
        "data": [
            {
                "id": "127.0.0.1",
                "address": "127.0.0.1",
                "appService": null,
                "connectionLimit": 0,
                "description": "a node",
                "dynamicRatio": 1,
                "fqdn": {
                    "addressFamily": "ipv4",
                    "autopopulate": "disabled",
                    "downInterval": 5,
                    "interval": 3600,
                    "name": "none"
                 },
                "logging": "disabled",
                "metadata": {},
                "monitorRule": {},
                "partition": "Common",
                "rateLimit": "disabled",
                "ratio": 1,
                "session": "user-enabled",
                "state": "unchecked"
            }
        ]
    }

Create a node
-------------

Add a node to the load balancer.

You can use the ``event ID`` returned in the API response to submit an event
request to verify that the operation completed and get the ID for the
new node.

::

    POST /nodes

**Request**

::

    {
        "address": "162.242.206.208",
        "appService": null,
        "connectionLimit": 2,
        "description": "test truncated",
        "dynamicRatio": 11,
        "logging": "enabled",
        "rateLimit": "disabled",
        "ratio": 1
    }

Response
^^^^^^^^

The node was created successfully.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "Nodes",
            "timestamp": "2016-03-08T17:22:33.6249648Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Retrieve node statistics
------------------------

Retrieve statistics for all nodes added to the load balancer.

You can use the links in the response to retrieve information about a specific
node.

::

    GET /nodes/stats

This operation does not accept a request body.

Response
^^^^^^^^
::

    {
        "data": [
            {
                "id": "my-special-node",
                "address": "127.0.0.1",
                "curSessions": 1,
                "monitorRule": {
                    "monitors": [
                        "default"
                    ],
                    "minimum": "all"
                },
                "serverside": {
                    "bitsIn": 1,
                    "bitsOut": 1,
                    "curConns": 1,
                    "maxConns": 2,
                    "pktsIn": 1,
                    "pktsOut": 1,
                    "totConns": 1
                },
                "sessionStatus": "enabled",
                "status": {
                    "availabilityState": "offline",
                    "enabledState": "enabled",
                    "statusReason": "Forced down"
                },
                "totRequests": 3
            }
        ]
    }

Retrieve node information by node ID
-------------------------------------

Retrieve information about the node associated with the node ID.

::

    GET /nodes/{nodeId}

*This operation does not accept a request body.*

Response
^^^^^^^^

::

    {
        "data": [
            {
                "id": "127.0.0.1",
                "address": "127.0.0.1",
                "appService": "none",
                "connectionLimit": 0,
                "description": "a node",
                "dynamicRatio": 1,
                "fqdn": {
                    "addressFamily": "ipv4",
                    "autopopulate": "disabled",
                    "downInterval": 5,
                    "interval": 3600,
                    "name": "none"
                },
                "logging": "disabled",
                "monitorRule": {},
                "metadata": {},
                "partition": "Common",
                "rateLimit": "disabled",
                "ratio": 1,
                "session": "user-enabled",
                "state": "unchecked"
            }
        ]
    }

Update a node
-------------

Change description and configuration settings for an
existing node. You need the node ID to complete this operation.

::

    PUT /nodes/{nodeId}

Request body
^^^^^^^^^^^^

::

    {
        "appService": null,
        "connectionLimit": 2,
        "description": "Updated node",
        "dynamicRatio": 11,
        "logging": "enabled",
        "rateLimit": "disabled",
        "ratio": 1
    }

Response
^^^^^^^^

The node was successfully updated.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<nodeId:str>",
            "timestamp": "2016-03-08T17:22:33.6249648Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Delete a node
-------------

Remove a node from the load balancer configuration. You need
the node ID to complete this operation.

::

    DELETE /nodes/{nodeId}

*This operation does not accept a request body.*

Response
^^^^^^^^

The node was successfully deleted.

::

    {
      "data": {
        "eventId": "<eventId:str>",
        "resource": "<nodeId:str>",
        "status": "PROCESSING",
        "timestamp": "2016-03-08T17:22:33.6349648Z",
        "eventRef": "/events/<eventId:str>"
      }
    }

Retrieve node statistics
------------------------

Retrieve information about availability, session status,
monitor rules for the device with the specified node ID.

::

    GET /nodes/{nodeId}/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

Returns statistics for the specified node.

::

    {
        "data": [
            {
                "id": "my-special-node",
                "address": "127.0.0.1",
                "curSessions": 1,
                "monitorRule": {
                    "monitors": [
                        "default"
                    ],
                    "minimum": "all"
                },
                "serverside": {
                    "bitsIn": 1,
                    "bitsOut": 1,
                    "curConns": 1,
                    "maxConns": 2,
                    "pktsIn": 1,
                    "pktsOut": 1,
                    "totConns": 1
                },
                "sessionStatus": "enabled",
                "status": {
                    "availabilityState": "offline",
                    "enabledState": "enabled",
                    "statusReason": "Forced down"
                },
                "totRequests": 3
            }
        ]
    }

Disable node for maintenance
----------------------------

This setting allows the node (all services on an IP address) to accept only
new connections that match an existing persistence session.
Use this feature to prevent new connections to a node without affecting
existing client connections on the same node.

To re-enable the node, see: `Enable node after maintenance`_.

.. note::

   It is important to understand differences between :ref:`disable-vs-offline`.

::

    PUT /nodes/{node_ID}

Request body
^^^^^^^^^^^^

::

    {
        "session": "user-disabled"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }


Enable node after maintenance
-----------------------------

This setting allows the node (all services on an IP address) to accept new
connections.
Use this feature to enable a node after maintenance.

::

    PUT /nodes/{nodeId}

Request body
^^^^^^^^^^^^

::

    {
        "session": "user-enabled"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }


Offline node for maintenance
----------------------------

This setting forces a node offline and allows only active connections.

To bring the node back online, see: `Online node after maintenance`_.

.. note::

   It is important to understand differences between :ref:`disable-vs-offline`.

Request body
^^^^^^^^^^^^

::

    {
        "state": "user-down"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }


Online node after maintenance
-----------------------------

Use this setting to bring a node back online, usually after maintenance.

Request body
^^^^^^^^^^^^

::

    {
        "state": "user-up"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }


Monitors
~~~~~~~~

Monitors verify the health and availability of a node, pool, or group of
nodes in a pool.

.. contents::
     :depth: 1
     :local:


Retrieve monitor rule for node
------------------------------

Retrieve information about the monitor rule applied to a specific node.

::

    GET /nodes/{nodeId}/monitor-rule

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
        "data": [
            {
                "names": [
                    "https_443",
                    "real_server",
                    "tcp_echo"
                ],
                "minimum": 1
            }
        ]
    }

Update a monitor rule on node
-----------------------------

Update the monitor rule configured for a specified node.

::

    PUT /nodes/{nodeId}/monitor-rule

Request body
^^^^^^^^^^^^

::

    {
        "names": [
            "https_443",
            "real_server",
            "tcp_echo"
        ],
        "minimum": 1
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<nodeId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Add a monitor rule to automate checks
-------------------------------------

Apply a monitor rule to the specified node.
To find the names of the available monitors, submit
a **GET monitors** request.

::

    POST /nodes/{nodeId}/monitor-rule

**Request body**

::

    {
        "names": [
            "https_443"
        ],
        "minimum": 1
    }

Response
^^^^^^^^

::

    {
      "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "eventRef": "/events/<eventId:str>",
        "timestamp": "2016-03-18T03:18:35.5077939Z"
      }
    }

Remove a monitor rule from a node
---------------------------------

Remove the monitor rule from the specified node.

.. note::

   This operation does not remove the monitor from the load balancer
   configuration.

   When you delete a monitor rule, the system deletes all monitors
   associated to the node as well.

::

    DELETE /nodes/{nodeId}/monitor-rule

Response
^^^^^^^^

Delete the monitor rule from the specified node.

::

    {
        "data" : {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }


Pools
~~~~~

Pools are customizable containers configured on load balancers to
specify the back-end devices (nodes) for managing web traffic. Each pool
can contain zero or more nodes, known as a pool member. You can bind pools
to one or more virtual servers.

Use the following operations to view and manage pools.

.. contents::
     :depth: 1
     :local:

Retrieve pools
--------------

Retrieve information about all pools created in the current load balancer.

::

    GET /pools

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
        "data": [
            {
                "id": "POOL-127.0.0.1-80",
                "allowNat": "yes",
                "allowSnat": "yes",
                "appService": null,
                "gatewayFailsafeDevice": null,
                "ignorePersistedWeight": "disabled",
                "ipTosToClient": "pass-through",
                "ipTosToServer": "pass-through",
                "linkQosToClient": "pass-through",
                "linkQosToServer": "pass-through",
                "loadBalancingMode": "round-robin",
                "metadata": [],
                "minActiveMembers": 0,
                "minUpMembers": 0,
                "minUpMembersAction": "failover",
                "minUpMembersChecking": "disabled",
                "partition": "Common",
                "profiles": null,
                "queueDepthLimit": 0,
                "queueOnConnectionLimit": "disabled",
                "queueTimeLimit": 0,
                "reselectTries": 0,
                "serviceDownAction": null,
                "slowRampTime": 10,
                "description": null,
                "members": {},
                "monitorRule": {}
            }
        ]
    }

Retrieve pool statistics
------------------------

Retrieve statistics for all associated pools in a load balancer.

::

    GET /pools/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve a list of stats.

::

    {
      "data": [
        {
          "id": "POOL-127.0.0.1-80",
          "activeMemberCnt": 1,
          "connq": {
            "ageEdm": 0,
            "ageEma": 0,
            "ageHead": 0,
            "ageMax": 0,
            "depth": 0,
            "serviced": 0
          },
          "connqAll": {
            "ageEdm": 0,
            "ageEma": 0,
            "ageHead": 0,
            "ageMax": 0,
            "depth": 0,
            "serviced": 0
          },
          "curSessions": 0,
          "minActiveMembers": 0,
          "monitorRule": {
            "monitors": [
              "MON-TCP-80"
            ],
            "minimum": "all"
          },
          "name": "POOL-127.0.0.1-80",
          "totRequests": 0,
          "serverside": {
            "bitsIn": 0,
            "bitsOut": 0,
            "curConns": 0,
            "maxConns": 0,
            "pktsIn": 0,
            "pktsOut": 0,
            "totConns": 0
          },
          "status": {
            "availabilityState": "available",
            "enabledState": "enabled",
            "statusReason": "The pool is available"
          }
        }
      ]
    }



Retrieve a pool by ID
---------------------

Retrieve information about a specified pool by using the pool ID.
Use the **Retrieve pools** operation to retireve all pools.

::

    GET /pools/{poolId}

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve the pool specified.

::

    {
        "data": [
            {
                "id": "POOL-127.0.0.1-80",
                "allowNat": "yes",
                "allowSnat": "yes",
                "appService": null,
                "gatewayFailsafeDevice": null,
                "ignorePersistedWeight": "disabled",
                "ipTosToClient": "pass-through",
                "ipTosToServer": "pass-through",
                "linkQosToClient": "pass-through",
                "linkQosToServer": "pass-through",
                "loadBalancingMode": "round-robin",
                "metadata": [],
                "minActiveMembers": 0,
                "minUpMembers": 0,
                "minUpMembersAction": "failover",
                "minUpMembersChecking": "disabled",
                "partition": "Common",
                "profiles": null,
                "queueDepthLimit": 0,
                "queueOnConnectionLimit": "disabled",
                "queueTimeLimit": 0,
                "reselectTries": 0,
                "serviceDownAction": null,
                "slowRampTime": 10,
                "description": "none",
                "members": {},
                "monitor": {},
                "monitorRule": {
                    "minimum": 1,
                    "names": [
                        "tcp"
                    ]
                }
            }
        ]
    }

Update a pool
-------------

Update the configuration for a specified pool.

::

    PUT /pools/{poolId}

*This operation does not accept a request body.*

Request body
^^^^^^^^^^^^

::

    {
        "allowNat": "yes",
        "allowSnat": "yes",
        "appService": null,
        "description": null,
        "gatewayFailsafeDevice": null,
        "ignorePersistedWeight": "disabled",
        "ipTosToClient": "pass-through",
        "ipTosToServer": "pass-through",
        "linkQosToClient": "pass-through",
        "linkQosToServer": "pass-through",
        "loadBalancingMode": "round-robin",
        "minActiveMembers": 0,
        "minUpMembers": 0,
        "minUpMembersAction": "failover",
        "minUpMembersChecking": "disabled",
        "profiles": null,
        "queueDepthLimit": 0,
        "queueOnConnectionLimit": "disabled",
        "queueTimeLimit": 0,
        "reselectTries": 0,
        "serviceDownAction": null,
        "slowRampTime": 10
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-24T10:41:08.6194067Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Remove a pool
-------------

Remove a specified pool from the load balancer configuration.

::

    DELETE /pools/{poolId}

*This operation does not accept a request body.*


Response
^^^^^^^^

Delete a pool specified by using a Pool ID.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "eventRef": "/events/<eventId:str>",
            "timestamp": "2016-03-24T10:41:08.6194067Z"
        }
    }

Retrieve a monitor rule for a pool
----------------------------------

Retrieve a monitor rule associated with a specified pool.

::

   GET /pools/{poolId}/monitor-rule

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve the monitor-rule specified.

    ::

        {
            "data": [
                {
                    "names": [
                        "https_443",
                        "real_server",
                        "tcp_echo"
                    ],
                    "minimum": 1
                }
            ]
        }

Update a monitor rule for a pool
--------------------------------

Update the monitor rule applied to a specified pool. Use the **retrieve
monitors by pool ID** operation to find the monitor rule name.

::

   PUT /pools/{poolId}/monitor-rule

Request body
^^^^^^^^^^^^

::

   {
      "names": [
         "tcp"
         ],
      "minimum": "all"
   }

Response
^^^^^^^^
::

   {
      "data": {
      "eventId": "<eventId:str)",
      "status": "PROCESSING",
      "resource": "<poolId:str>",
      "timestamp": "2016-03-16T17:09:53.1059638Z",
      "eventRef": "/events/<eventId:str>"
      }
   }

Add a monitor rule to a pool
----------------------------

Add a monitor rule to a specified pool. To find the names of the available
monitors, submit a **GET monitors** request.

::

   POST /pools/{poolId}/monitor-rule

Request body
^^^^^^^^^^^^
::

   {
      "names": [
         "tcp"
      ],
      "minimum": 1
   }

Response
^^^^^^^^
::

    {
        "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "timestamp": "2016-03-18T03:18:35.5077939Z",
        "resource": "<poolId:str>",
        "eventRef": "/events/<eventId:str>"
        }
    }

Remove monitor rule from a pool
--------------------------------

Delete a monitor rule for the specified pool.

.. note::
   When you delete a monitor-rule, the system deletes all monitors associated
   to the pool as well.

::


   DELETE /pools/{poolId}/monitor-rule

*This operation does not accept a request body.*

Response
^^^^^^^^
   ::

      {
         "data": {
            "eventId": "<eventId:str]",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-16T17:09:53.1059638Z",
            "eventRef": "/events/<eventId:str>"
         }
      }

Retrieve pool member statistics for a pool
------------------------------------------

Retrieve statistics for each pool member in a specified pool, including
configuration settings, availability, and monitoring status. The response
includes links to access a detail view for each member.

::

   GET /pools/{poolId}/stats


*This operation does not accept a request body.*

Response
^^^^^^^^^

.. code::

      {
         "data": [
            {
               "id": "test1:80",
               "activeMemberCnt": 0,
               "address": "127.0.0.1",
               "connq": {
                   "ageEdm": 0,
                   "ageEma": 0,
                   "ageHead": 0,
                   "ageMax": 0,
                   "depth": 0,
                   "serviced": 0
                },
               "connqAll": {
                    "ageEdm": 0,
                    "ageEma": 0,
                    "ageHead": 0,
                    "ageMax": 0,
                    "depth": 0,
                    "serviced": 0
               },
               "curSessions": 0,
               "minActiveMembers": 0,
               "monitorRule": {
                   "monitors": [
                   "default"
                   ],
                   "minimum": "all"
               },
               "name": "test1:80",
               "serverside": {
                   "bitsIn": 0,
                   "bitsOut": 0,
                   "curConns": 0,
                   "maxConns": 0,
                   "pktsIn": 0,
                   "pktsOut": 0,
                   "totConns": 0
               },
               "status": {
                   "availabilityState": "unknown",
                   "enabledState": "enabled",
                   "statusReason": "Pool member does not have service checking enabled"
               },
               "totRequests": 0
            }
         ]
      }



Pool members
~~~~~~~~~~~~

Pool members are logical, physical objects representing a single internal
physical server IP address and listener port. You assign pool members to
pools and use them to load balance traffic directed to the pool associated with
a virtual server configured in the load balancer.

Use the following operations to view and manage pool members.

.. contents::
     :depth: 1
     :local:


Retrieve pool members for a pool
--------------------------------

Retrieve a list of members associated with a specific pool ID.

::

    GET /pools/{poolId}/members

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
      "data": [
        {
          "id": "127.0.0.1:80",
          "port": {
            "type": "equal",
            "value": 80
          },
          "monitorRule": {},
          "address": "127.0.0.1",
          "appService": null,
          "connectionLimit": 0,
          "description": "none",
          "dynamicRatio": 1,
          "inheritProfile": "enabled",
          "logging": "disabled",
          "priorityGroup": 0,
          "rateLimit": "disabled",
          "ratio": 1,
          "session": "monitor-enabled",
          "state": "unchecked",
          "metadata": {},
          "profiles": null
        }
      ]
    }

Create a pool member in a pool
-------------------------------

Create a pool member by adding an existing node to a
specified pool.

::

    POST /pools/{poolId}/members

Request body
^^^^^^^^^^^^
::

    {
        "nodeId": "<nodeId>",
        "port": {
            "type": "equal",
            "value": 80
        }
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "resource": "<poolId:str>",
            "status": "PROCESSING",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

.. _retrieve-statistics:

Retrieve statistics for pool members
-------------------------------------

Retrieve statistics for all pool members in a specified pool, including
configuration settings, availability, and monitoring status.

::

    GET /pools/{poolId}/members/stats

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
      "data": [
        {
          "id": "test1:80",
          "address": "127.0.0.1",
          "connq": {
            "ageEdm": 0,
            "ageEma": 0,
            "ageHead": 0,
            "ageMax": 0,
            "depth": 0,
            "serviced": 0
          },
          "curSessions": 0,
          "monitorRule": {
            "monitors": [
              "default"
            ],
            "minimum": "all"
          },
          "monitorStatus": "address-down",
          "nodeName": "test1",
          "poolName": "test2",
          "port": {
            "type": "equal",
            "value": 80
          },
          "serverside": {
            "bitsIn": 0,
            "bitsOut": 0,
            "curConns": 0,
            "maxConns": 0,
            "pktsIn": 0,
            "pktsOut": 0,
            "totConns": 0
          },
          "sessionStatus": "enabled",
          "status": {
            "availabilityState": "unknown",
            "enabledState": "enabled",
            "statusReason": "Pool member does not have service checking enabled"
          },
          "totRequests": 0
        }
      ]
    }

Retrieve pool member configuration
----------------------------------

Retrieve configuration, monitor settings, and other data for a pool member.

::

    GET /pools/{poolId}/members/{memberId}

*This operation does not accept a request body.*

Response
^^^^^^^^

::

    {
        "data": [
            {
                "id": "127.0.0.1:80",
                "address": "127.0.0.1",
                "appService": null,
                "connectionLimit": 0,
                "description": null,
                "dynamicRatio": 1,
                "inheritProfile": "enabled",
                "logging": "disabled",
                "priorityGroup": 0,
                "port": {
                    "type": "equal",
                    "value": 70
                },
                "rateLimit": "disabled",
                "ratio": 1,
                "session": "user-enabled",
                "state": "unchecked",
                "metadata": {},
                "monitorRule": {},
                "profiles": []
            }
        ]
    }

Update pool member configuration
--------------------------------

Update configuration settings for a specified pool member.

::

    PUT /pools/{poolId}/members/{memberId}

Request body
^^^^^^^^^^^^^
::

    {
        "appService": null,
        "connectionLimit": 0,
        "description": null,
        "dynamicRatio": 1,
        "inheritProfile": "enabled",
        "logging": "enabled",
        "priorityGroup": 0,
        "rateLimit": "enabled"
     }

Response
^^^^^^^^

Update a pool member by pool ID.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Remove a pool member from pool
------------------------------

Remove a pool member by a specified pool ID.

::

    DELETE /pools/{poolId}/members/{memberId}


*This operation does not accept a request body.*


Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Retrieve a pool member monitor rule
-----------------------------------

Retrieves configuration settings for a monitor
rule applied to a specified pool member.

::

    GET /pools/{poolId}/members/{memberId}/monitor-rule

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
      "data": [
        {
          "minimum": "all",
          "names": [
                "default"
          ]
        }
      ]
    }

Update a monitor rule for pool member
-------------------------------------

Update the configuration settings for the monitor rule applied to a specified
pool member.

::

    PUT /pools/{poolId}/members/{memberId}/monitor-rule

Request body
^^^^^^^^^^^^

::

    {
        "names": [
            "tcp"
        ],
        "minimum": 1
    }

Response
^^^^^^^^

Returns event information for the update monitor rule request. Use the
event ID to get event status and output information.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-16T17:09:53.1059638Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Create a monitor rule for a pool member
---------------------------------------

Add monitors rule to a pool member in a specified pool.

::

    POST /pools/{poolId}/members/{memberId}/monitor-rule

Request body
^^^^^^^^^^^^^

::

    {
      "names": [
        "tcp",
        "https"
      ],
      "minimum": 1
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-24T10:41:08.6194067Z",
            "eventRef": "/events/<eventId:str>"
        }
    }


Remove a monitor rule from a pool member
----------------------------------------

Remove the monitor rule applied to a specified
pool member (``memberId``) in a specified pool (``poolId``).

::

    DELETE /pools/{poolId}/members/{memberID}/monitor-rule

Response
^^^^^^^^

Returns event information for the update monitor rule request. Use the
event ID to retrieve event status and output information.

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "resource": "<poolId:str>",
            "eventRef": "/events/<eventId:str}",
            "status": "PROCESSING",
            "timestamp": "2016-03-08T17:22:33.6249648Z"
        }
    }

Retrieve statistics for a pool member
-------------------------------------

Retrieve configuration, monitor settings, and other data for a pool member.

::

    GET /pools/{poolId}/members/{memberId}/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

::

    {
        "data": [
            {
                "id": "test1:80",
                "address": "127.0.0.1",
                "connq": {
                    "ageEdm": 0,
                    "ageEma": 0,
                    "ageHead": 0,
                    "ageMax": 0,
                    "depth": 0,
                    "serviced": 0
                },
                "curSessions": 0,
                "monitorRule": {
                    "monitors": [
                        "default"
                    ],
                    "minimum": "all"
                },
                "monitorStatus": "address-down",
                "nodeName": "test1",
                "poolName": "test2",
                "port": {
                    "type": "equal",
                    "value": 80
                },
                "serverside": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "sessionStatus": "enabled",
                "status": {
                    "availabilityState": "unknown",
                    "enabledState": "enabled",
                    "statusReason": "Pool member does not have service checking enabled"
                },
                "totRequests": 0
            }
        ]
    }

Disable a pool member for maintenance
-------------------------------------

This setting allows the pool member (a combination of IP and port) to accept
only new connections that match an existing persistence session.
Use this feature to prevent new connections to a pool member without
affecting existing client connections on the same pool member.

To monitor connection stats of a pool member, see:
:ref:`retrieve-statistics`. Review the first object in the
data array.
The ``serverside`` object shows stats on activity to the member.

To re-enable the pool member, see :ref:`enable-pool-member`.

.. note::

   It is important to understand differences between :ref:`disable-vs-offline`.

::

    PUT /pools/{pool_ID}/members/{member_ID}

Request body
^^^^^^^^^^^^

::

    {
        "session": "user-disabled"
    }

Response
^^^^^^^^

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

.. _enable-pool-member:

Enable a pool member after maintenance
--------------------------------------

This setting allows the pool member (a combination of IP and Port) to accept
new connections.
Use this feature to enable a pool member after maintenance.

::

    PUT /pools/{pool_ID}/members/{member_ID}

Request body
^^^^^^^^^^^^

::

    {
        "session": "user-enabled"
    }

Response
^^^^^^^^

::

   {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<poolId:str>",
            "timestamp": "2016-03-17T09:36:42.5274609Z",
            "eventRef": "/events/<eventId:str>"
        }
    }


Offline a pool member for maintenance
-------------------------------------

This setting enables you to force a pool member offline and allows only active
connections.

.. note::

   It is important to understand differences between :ref:`disable-vs-offline`.

Request body
^^^^^^^^^^^^

::

    {
        "state": "user-down"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }


Online a pool member after maintenance
--------------------------------------

Use this setting to bring a pool member back online, usually after maintenance.

Request body
^^^^^^^^^^^^

::

    {
        "state": "user-up"
    }

Response
^^^^^^^^

::

    {
    "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<nodeId:str>",
        "timestamp": "2016-03-08T17:22:33.6249648Z",
        "eventRef": "/events/<eventId:str>"
        }
    }



Virtual servers
~~~~~~~~~~~~~~~

Virtual servers are combination of an IP and a port that distribute traffic
among nodes in a pool. You can associate a virtual server with one or more
pools.

Use the following operations to view and manage virtual servers configured in
the load balancer.

.. contents::
     :depth: 1
     :local:

Retrieve virtual server details
-------------------------------

Retrieve information about all virtual servers configured in the load
balancer, including configuration data and status information.

::

    GET /virtuals

*This operation does not accept a request body.*

Response
^^^^^^^^

::

    {
        "data": [
            {
                "id": "VIP-127.0.0.1-80",
                "address": "127.0.0.1",
                "addressStatus": "yes",
                "appService": null,
                "auth": {},
                "autoLasthop": "default",
                "bwcPolicy": null,
                "clonePools": {},
                "cmpEnabled": "yes",
                "connectionLimit": 0,
                "description": null,
                "fallbackPersistence": null,
                "gtmScore": 0,
                "ipForward": "",
                "ipProtocol": "tcp",
                "lastHopPool": null,
                "mask": "255.255.255.255",
                "metadata": {},
                "mirror": "disabled",
                "mobileAppTunnel": "disabled",
                "nat64": "disabled",
                "partition": "Common",
                "persist": {
                    "cookie": {
                        "default": "yes"
                    }
                },
                "policies": "none",
                "pool": {},
                "port": {
                    "type": "equal",
                    "value": 80
                },
                "profiles": {
                    "http": {
                        "context": "all"
                    },
                    "tcp": {
                        "context": "all"
                    }
                },
                "rateClass": null,
                "rateLimit": "disabled",
                "rateLimitDstMask": 0,
                "rateLimitMode": "object",
                "rateLimitSrcMask": 0,
                "relatedRules": null,
                "rules": null,
                "securityLogProfiles": {},
                "source": "0.0.0.0/0",
                "sourceAddressTranslation": {
                    "pool": "none",
                    "type": "none"
                },
                "sourcePort": "preserve",
                "synCookieStatus": "not-activated",
                "trafficClasses": {},
                "translateAddress": "enabled",
                "translatePort": "enabled",
                "vlans": {},
                "vsIndex": 7
            }
        ]
    }

Add a virtual server
--------------------

Add a virtual server configuration to the load balancer. When you
add a virtual server configuration, do not specify an IP address unless you
want to add a configuration to an existing address on a unique port.

::

    POST /virtuals

Request body
^^^^^^^^^^^^

::

    {
      "address": "172.16.1.160",
      "source": "0.0.0.0\/0",
      "ipProtocol": "tcp",
      "ipForward": "disabled",
      "gtmScore": 0,
      "description": "New Description",
      "fallbackPersistence": null,
      "port": {
        "value": 80,
        "type": "equal"
      },
      "connectionLimit": 99
    }

Response
^^^^^^^^

Returns event information for the request. Use the event ID to get event
status and output information.

::

    {
      "data": {
        "eventId": "02d1ba2a-0edf-4583-8e2c-ab0b54c78193",
        "status": "PROCESSING",
        "resource": "Virtuals",
        "eventRef": "/events/<eventId:str>",
        "timestamp": "2016-03-18T03:18:35.5077939Z"
      }
    }

Retrieve virtual server statistics
-----------------------------------

Retrieve statistical information for all virtual servers configured in
the load balancer.

::

    GET /virtuals/stats

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
        "data": [
            {
                "clientside": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "cmpEnableMode": "all-cpus",
                "cmpEnabled": "enabled",
                "csMaxConnDur": 0,
                "csMeanConnDur": 0,
                "csMinConnDur": 0,
                "destination": "127.0.0.1:80",
                "ephemeral": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "fiveMinAvgUsageRatio": 0,
                "fiveSecAvgUsageRatio": 0,
                "id": "VIP-127.0.0.1-80",
                "name": "VIP-127.0.0.1-80",
                "oneMinAvgUsageRatio": 0,
                "status": {
                    "availabilityState": "unknown",
                    "enabledState": "enabled",
                    "statusReason": "The children pool member(s) either don't have service checking enabled, or service check results are not available yet"
                },
                "syncookie": {
                    "accepts": 0,
                    "hwAccepts": 0,
                    "hwSyncookies": 0,
                    "hwsyncookieInstance": 0,
                    "rejects": 0,
                    "swsyncookieInstance": 0,
                    "syncacheCurr": 0,
                    "syncacheOver": 0,
                    "syncookies": 0
                },
                "syncookieStatus": "not-activated",
                "totRequests": 0
            },
            {
                "clientside": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "cmpEnableMode": "all-cpus",
                "cmpEnabled": "enabled",
                "csMaxConnDur": 0,
                "csMeanConnDur": 0,
                "csMinConnDur": 0,
                "destination": "127.0.0.1:443",
                "ephemeral": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "fiveMinAvgUsageRatio": 0,
                "fiveSecAvgUsageRatio": 0,
                "id": "TestVip-DONT-DELETE",
                "name": "TestVip-DONT-DELETE",
                "oneMinAvgUsageRatio": 0,
                "status": {
                    "availabilityState": "unknown",
                    "enabledState": "enabled",
                    "statusReason": "The children pool member(s) either don't have service checking enabled, or service check results are not available yet"
                },
                "syncookie": {
                    "accepts": 0,
                    "hwAccepts": 0,
                    "hwSyncookies": 0,
                    "hwsyncookieInstance": 0,
                    "rejects": 0,
                    "swsyncookieInstance": 0,
                    "syncacheCurr": 0,
                    "syncacheOver": 0,
                    "syncookies": 0
                },
                "syncookieStatus": "not-activated",
                "totRequests": 0
            },
            {
                "clientside": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "cmpEnableMode": "all-cpus",
                "cmpEnabled": "enabled",
                "csMaxConnDur": 0,
                "csMeanConnDur": 0,
                "csMinConnDur": 0,
                "destination": "127.0.0.1:443",
                "ephemeral": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "fiveMinAvgUsageRatio": 0,
                "fiveSecAvgUsageRatio": 0,
                "id": "VIP-127.0.0.1-443",
                "name": "VIP-127.0.0.1-443",
                "oneMinAvgUsageRatio": 0,
                "status": {
                    "availabilityState": "available",
                    "enabledState": "enabled",
                    "statusReason": "The virtual server is available"
                },
                "syncookie": {
                    "accepts": 0,
                    "hwAccepts": 0,
                    "hwSyncookies": 0,
                    "hwsyncookieInstance": 0,
                    "rejects": 0,
                    "swsyncookieInstance": 0,
                    "syncacheCurr": 0,
                    "syncacheOver": 0,
                    "syncookies": 0
                },
                "syncookieStatus": "not-activated",
                "totRequests": 0
            },
            {
                "clientside": {
                    "bitsIn": 2784874696,
                    "bitsOut": 13416053656,
                    "curConns": 5,
                    "maxConns": 61,
                    "pktsIn": 5698557,
                    "pktsOut": 1560895,
                    "totConns": 1485109
                },
                "cmpEnableMode": "all-cpus",
                "cmpEnabled": "enabled",
                "csMaxConnDur": 14319373760,
                "csMeanConnDur": 7972,
                "csMinConnDur": 56,
                "destination": "any:any",
                "ephemeral": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "fiveMinAvgUsageRatio": 0,
                "fiveSecAvgUsageRatio": 0,
                "id": "VS-FORWARDING",
                "name": "VS-FORWARDING",
                "oneMinAvgUsageRatio": 0,
                "status": {
                    "availabilityState": "unknown",
                    "enabledState": "enabled",
                    "statusReason": "The children pool member(s) either don't have service checking enabled, or service check results are not available yet"
                },
                "syncookie": {
                    "accepts": 0,
                    "hwAccepts": 0,
                    "hwSyncookies": 0,
                    "hwsyncookieInstance": 0,
                    "rejects": 2,
                    "swsyncookieInstance": 0,
                    "syncacheCurr": 0,
                    "syncacheOver": 0,
                    "syncookies": 0
                },
                "syncookieStatus": "not-activated",
                "totRequests": 0
            }
        ]
    }

Retrieve virtual server information by ID
-----------------------------------------

Retrieve information about the specified virtual server.

::

    GET /virtuals/{virtualId}

*This operation does not accept a request body.*

Response
^^^^^^^^

::

    {
        "data": [
            {
                "id": "VIP-127.0.0.1-80",
                "address": "127.0.0.1",
                "addressStatus": "yes",
                "appService": "none",
                "auth": {},
                "autoLasthop": "default",
                "bwcPolicy": null,
                "clonePools": {},
                "cmpEnabled": "yes",
                "connectionLimit": 0,
                "description": "none",
                "fallbackPersistence": null,
                "gtmScore": 0,
                "ipForward": "",
                "ipProtocol": "tcp",
                "lastHopPool": null,
                "mask": "255.255.255.255",
                "metadata": null,
                "mirror": "disabled",
                "mobileAppTunnel": "disabled",
                "nat64": "disabled",
                "partition": "Common",
                "persist": {
                    "cookie": {
                        "default": "yes"
                    }
                },
                "policies": {},
                "pool": {},
                "port": {
                    "type": "equal",
                    "value": 80
                },
                "profiles": {
                    "http": {
                        "context": "all"
                    },
                    "tcp": {
                        "context": "all"
                    }
                },
                "rateClass": null,
                "rateLimit": "disabled",
                "rateLimitDstMask": 0,
                "rateLimitMode": "object",
                "rateLimitSrcMask": 0,
                "relatedRules": null,
                "rules": null,
                "securityLogProfiles": {},
                "source": "0.0.0.0/0",
                "sourceAddressTranslation": {
                    "pool": "none",
                    "type": "none"
                },
                "sourcePort": "preserve",
                "synCookieStatus": "not-activated",
                "trafficClasses": {},
                "translateAddress": "enabled",
                "translatePort": "enabled",
                "vlans": {},
                "vsIndex": 7
            }
        ]
    }

Update a virtual server by ID
-----------------------------

Update the virtual server on a specified device by using the virtual ID.

When you update an existing virtual server, you must specify the address and
port in the request.

::

    PUT /virtuals/{virtualId}

Request body
^^^^^^^^^^^^^

::

    {
        "address": "172.16.1.160",
        "source": "0.0.0.0\/0",
        "ipProtocol": "tcp",
        "ipForward": "disabled",
        "gtmScore": 0,
        "description": "New Description updated",
        "fallbackPersistence": null,
        "port": {
            "value": 80,
            "type": "equal"
        },
        "connectionLimit": 99
    }

Response
^^^^^^^^

Returns event information for the request. Use the event ID to get event
status and output information.

::

    {
        "data": {
            "eventId": "02d1ba2a-0edf-4583-8e2c-ab0b54c78193",
            "status": "PROCESSING",
            "resource": "<virtualId:str>",
            "eventRef": "/events/<eventId:str>",
            "timestamp": "2016-03-18T03:18:35.5077939Z"
        }
    }

Remove a virtual server
-----------------------

Remove a specified virtual server from the load balancer configuration.

::

    DELETE /virtuals/{virtualId}

*This operation does not accept a request body.*

Response
^^^^^^^^

Returns event information for the request. Use the event ID to get event
status and output information.

::

    {
        "data": {
            "eventId": "<eventid:str>",
            "status": "PROCESSING",
            "resource": "<virtualId:str>",
            "timestamp": "2016-03-18T03:18:35.5077939Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Retrieve persistent profiles for a virtual server
-------------------------------------------------

Retrieve information about the persistent profiles configured for a virtual
server. These profiles enable tracking and storing session data to ensure
that client requests are directed to the same pool member throughout the life
of a session or during subsequent sessions.

::

    GET /virtuals/{virtualId}/persists

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
        "data": [
            {
              "profileName": "my-cool-persist"
            }

        ]
    }

Update a virtual server persistent profile
------------------------------------------

Update the persistent profile for a virtual server.

::

    PUT /virtuals/{virtualId}/persists

Request body
^^^^^^^^^^^^

::

    {
        "names": [
        "hash"
        ]
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<virtualId:str>",
            "timestamp": "2016-03-08T17:22:33.6249648Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Create a persistent profile
---------------------------

Create a persistent profile configuration for a specified
virtual server.

::

    POST /virtuals/{virtualId}/persists


*This operation does not accept a request body.*

Request body
^^^^^^^^^^^^

::

    {
        "names": [
            "source_addr",
            "dest_addr"
        ]
    }

Response
^^^^^^^^

::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<virtualId:str>",
            "timestamp": "2016-03-08T17:22:33.6249648Z",
            "eventRef": "/events/<eventId:str>"
        }
    }

Remove a persistent profile
----------------------------

Remove a persistent profile configuration from a specified virtual server.

::

    DELETE /virtuals/{virtualId}/persists

*This operation does not accept a request body.*

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<virtualId:str>",
            "eventRef": "/events/<eventId:str>",
            "timestamp": "2016-03-18T03:18:35.5077939Z"
        }
    }

Retrieve virtual server information by ID
-----------------------------------------

Retrieve statistics for a specified virtual server configured in the load
balancer.

::

    GET /virtuals/{virtualId}/stats

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve a list of stats.

::

    {
        "data": [
            {
                "clientside": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "cmpEnableMode": "all-cpus",
                "cmpEnabled": "enabled",
                "csMaxConnDur": 0,
                "csMeanConnDur": 0,
                "csMinConnDur": 0,
                "destination": "127.0.0.1:80",
                "ephemeral": {
                    "bitsIn": 0,
                    "bitsOut": 0,
                    "curConns": 0,
                    "maxConns": 0,
                    "pktsIn": 0,
                    "pktsOut": 0,
                    "totConns": 0
                },
                "fiveMinAvgUsageRatio": 0,
                "fiveSecAvgUsageRatio": 0,
                "id": "VIP-127.0.0.1-80",
                "name": "VIP-127.0.0.1-80",
                "oneMinAvgUsageRatio": 0,
                "status": {
                    "availabilityState": "unknown",
                    "enabledState": "enabled",
                    "statusReason": "The children pool member(s) either don't have service checking enabled, or service check results are not available yet"
                },
                "syncookie": {
                    "accepts": 0,
                    "hwAccepts": 0,
                    "hwSyncookies": 0,
                    "hwsyncookieInstance": 0,
                    "rejects": 0,
                    "swsyncookieInstance": 0,
                    "syncacheCurr": 0,
                    "syncacheOver": 0,
                    "syncookies": 0
                },
                "syncookieStatus": "not-activated",
                "totRequests": 0
            }
        ]
    }


Retrieve virtual pools by virtual ID
------------------------------------

Retrieve information about the virtual pools associated with a specified
virtual server.

::

    GET /virtuals/{virtualId}/pool

*This operation does not accept a request body.*

Response
^^^^^^^^^
::

    {
        "data": [
            {
                "name": "test_pool"
            }
        ]
    }


Monitors
~~~~~~~~

Monitors verify the health and availability of a node, a pool, or a group of
nodes in a pool.

Use the following operations to view and manage monitors and monitor
configuration in the load balancer.

.. contents::
     :depth: 1
     :local:

Retrieve monitors
-----------------

Retrieve monitors configured in the load balancer.

::

    GET /monitors

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve a list of monitors.

::

    {
        "data": [
            {
                "acceptRcode": "no-error",
                "address": "any",
                "answerContains": "query-type",
                "appService": null,
                "defaultsFrom": null,
                "description": null,
                "id": "dns",
                "interval": 5,
                "manualResume": "disabled",
                "port": {
                    "type": "any",
                    "value": "any"
                },
                "qname": null,
                "qtype": "a",
                "recv": null,
                "reverse": "disabled",
                "timeUntilUp": 0,
                "timeout": 16,
                "transparent": "disabled",
                "type": "dns",
                "upInterval": 0
            },
            {
                "address": "any",
                "appService": null,
                "cert": null,
                "cipherlist": null,
                "compatibility": null,
                "defaultsFrom": null,
                "description": null,
                "id": "https",
                "interval": 5,
                "ipDscp": 0,
                "key": null,
                "manualResume": "disabled",
                "password": null,
                "port": {
                    "type": "any",
                    "value": "any"
                },
                "recv": null,
                "recvDisable": null,
                "reverse": "disabled",
                "send": "\"GET /\\r\\n\"",
                "timeUntilUp": 0,
                "timeout": 16,
                "transparent": "disabled",
                "type": "https",
                "upInterval": 0,
                "username": null
            },
            {
                "address": "1.2.3.27",
                "appService": null,
                "defaultsFrom": "tcp",
                "description": "\"Updated value\"",
                "id": "FakeTestMonitor",
                "interval": 5,
                "ipDscp": 0,
                "manualResume": "disabled",
                "port": {
                    "type": "equal",
                    "value": 86
                },
                "recv": "stuff",
                "recvDisable": "disabled",
                "reverse": "disabled",
                "send": null,
                "timeUntilUp": 0,
                "timeout": 16,
                "transparent": "enabled",
                "type": "tcp",
                "upInterval": 0
            }
        ]
    }

Retrieve a monitor by ID
------------------------

Retrieve information about a specified monitor by using the monitor ID.

::

    GET /monitors/{monitorId}

*This operation does not accept a request body.*

Response
^^^^^^^^

Retrieve details about a specified monitor.

::

    {
        "data": [
            {
                "id": "MON-TCP-80",
                "type": "tcp",
                "address":"any",
                "port": {
                    "type": "equal",
                    "value": 80
                },
                "appService": null,
                "defaultsFrom": "tcp",
                "description": "none",
                "interval": 5,
                "ipDscp": 0,
                "manualResume": "disabled",
                "recv": null,
                "recvDisable": null,
                "reverse": "disabled",
                "send": null,
                "timeUntilUp": 0,
                "timeout": 16,
                "transparent": "disabled",
                "upInterval": 0
            }
        ]
    }

Update a monitor
----------------

Update a specified monitor configured in the load balancer.


::

    PUT /monitors/{monitorId}

Request body
^^^^^^^^^^^^^

::

    {
        "address": "1.2.3.27",
        "port": {
            "type": "any",
            "value": "86"
        },
        "type": "tcp",
        "defaultsFrom": "/Common/tcp",
        "description": "Updated value",
        "interval": 5,
        "ipDscp": 0,
        "manualResume": "disabled",
        "recv": "stuff",
        "recvDisable": "disabled",
        "reverse": "disabled",
        "send": null,
        "timeUntilUp": 0,
        "timeout": 0,
        "transparent": "enabled",
        "upInterval": 0
    }

Response
^^^^^^^^

Update a monitor in the load balancer.

::


    {
        "data": {
            "eventId": "32d1ba2a-0edf-4583-8e2c-ab0b54c78193",
            "status": "PROCESSING",
            "resource": "<monitorId:str>",
            "eventRef": "/events/<eventId:str>",
            "timestamp": "2016-03-18T03:18:35.5077939Z"
        }
    }

Create a monitor
----------------

Add a monitor to the load balancer configuration.

::

    POST /monitors/{monitorId}

**Request**

::

    {
      "address": "1.2.3.27",
      "port": {
        "type": "any",
        "value": "85"
      },
      "type": "tcp",
      "defaultsFrom": "/Common/tcp",
      "description": "A updated peg tcp monitor",
      "interval": 5,
      "ipDscp": 0,
      "manualResume": "disabled",
      "recv": "stuff",
      "recvDisable": "disabled",
      "reverse": "disabled",
      "send": null,
      "timeUntilUp": 0,
      "timeout": 0,
      "transparent": "enabled",
      "upInterval": 0
    }

Response
^^^^^^^^
::

    {
        "data": {
            "eventId": "<eventId:str>",
            "status": "PROCESSING",
            "resource": "<monitorId:str>",
            "eventRef": "/events/<eventId:str>",
            "timestamp": "2016-03-18T03:18:35.5077939Z"
        }
    }

Remove a monitor from the load balancer
---------------------------------------

Remove a specified monitor from the load balancer configuration.

::

    DELETE /monitors/{monitorId}

*This operation does not accept a request body.*


Response
^^^^^^^^
::

    {
      "data": {
        "eventId": "<eventId:str>",
        "status": "PROCESSING",
        "resource": "<monitorId>",
        "timestamp": "2016-03-24T10:41:08.6194067Z",
        "eventRef": "/events/<eventId:str>"
      }
    }

Events
~~~~~~


Retrieve an event by event id
-----------------------------

Retrieve event information by using the event ID.

::

    GET /events/{eventId}

*This operation does not accept a request body.*

Response
^^^^^^^^

Returns information about the event with the specified ID.

::

    {
        "data": [{
            "event_id": "<eventId:str>",
            "status": "200",
            "message": "COMPLETED",
            "entrytimestamp": "2016-03-04T21:29:12",
            "modifiedtimestamp": "2016-03-04T21:29:12"
        }]
    }
