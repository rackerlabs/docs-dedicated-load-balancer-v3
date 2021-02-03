.. _concepts:

========
Concepts
========

To use the |apiservice| effectively, you should understand the following
terminology:


.. _load-balancer-concept:

Load balancer
~~~~~~~~~~~~~~~

A load balancer distributes workloads across two or more servers,
network links, or other resources based on criteria defined in the
load balancer configuration.

.. _virtuals-concept:

Virtual server
~~~~~~~~~~~~~~

A virtual server is an IP address (virtual IP) and service port combination,
for example ``172.16.200.210:80``. A virtual server's main function is to
distribute traffic loads to a group of internal, back-end servers. All traffic
is directed to the virtual server, and the server distributes the traffic
across the back-end devices.

Virtual servers are configured differently on |ADX| and |F5| devices.

|ADX| virtual server configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On an |ADX|, virtual servers are defined explicitly by binding
virtual servers with real server resources. A real
server is a configuration object existing within the ADX that defines the
IP address of the internal, back-end server. You associate server resources
with a virtual server by using the virtual server binding syntax as shown in
the following example:

.. list-table:: **Brocade ADX load balancer configuration**
   :widths: 20 50
   :header-rows: 1

   * - Logical component
     - Configuration
   * - Virtual server
     - server virtual VS-1.2.3.4 172.16.200.210
   * -   real server1 binding
     -   bind http web1 bind http web2
   * -   real server2 binding
     -   bind http web2

.. note::

   If a real server has multiple service ports configured, you can use those
   ports as resources for the virtual server.


|F5| virtual server configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On a |F5|, you configure the backend devices associated with a virtual server
by using :ref:`pools <pools-concept>` that specify the configuration for the
backend devices and :ref:`pool members <pool-member-concept>` for managing
traffic.

.. list-table:: **BIG-IP F5 load balancer configuration**
   :widths: 20 50
   :header-rows: 1

   * - Logical component
     - Configuration
   * - VIP
     - VS-1.2.3.4:80
   * -   Pool
     -   POOL-172.16.200.210-80
   * -     Member 1
     -     10.200.10.25:80
   * -     Member 2
     -     10.200.10.26:80
   * -     Member 3
     -     10.200.10.27:80

After a pool is applied to a virtual server, client traffic destined
for the virtual server uses the pool as its resource. For a pool to
serve its function of fault tolerance and redundancy, multiple pool members
must be available inside the pool. When a pool has multiple
members, the load balancer decides which one receives the client traffic based
on the load balancing method configured on the device (such as Round Robin, Weighted
Robin, or Least Connection).


.. _pools-concept:

Pools
~~~~~

On |F5|, a load balancing pool is a configuration object created in the Local
Traffic Management system. Each pool is a logical grouping of
devices used to receive and process traffic.

You can define multiple pools in the |F5| configuration. However,
you associate each pool with only one virtual server that
distributes traffic across the devices in the pool.

After you create a pool, you configure the backend resources in the pool by
adding pool members.

.. _pool-member-concept:

Pool member
~~~~~~~~~~~

A pool member is a logical object representing a single
internal physical server IP address and listener port. You can include a
pool member in multiple pools.

.. _node-concept:

Node
~~~~

A node is a logical object that provides the IP address of a single, physical
backend device, such as a web server. Nodes are the base configuration
object when creating a virtual server.

In an |F5|, the system automatically adds the node when you add a member to a pool.

.. _event-concept:

Event
~~~~~

You can use events to track asynchronous backend processing status. The system generates
these events when a user performs an action on the load balancer that 
updates a device. The returned response object includes an event ID,  
event type, status of the request, and the timestamp when the system created the event.

.. _monitor-concept:

Monitor
~~~~~~~

For a |F5|, monitors verify the health and availability of a node, pool, or a
group of nodes in a pool.

A health monitor, or a health check, is a configuration object that specifies
a check and parameter values to verify the health and status of a load
balancer component such as a pool. Checks run continuously at a time interval
specified in the monitor configuration. If the component does not
reply correctly to the health monitor before the timeout value expires, the
system identifies it as having degraded performance and removes that component
from the availability pool.

A monitor does not take effect until you apply it to a virtual server,
a pool, or a pool member. You apply it by submitting an API request to
create a monitor rule. After applying a rule, you can use the update monitor
rule operations to change configuration settings.

On the |F5|, the default interval timer is five seconds, with a default timeout
value of 16 seconds.
