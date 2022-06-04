## EIGRP Quick Info

* Routing Protocol Type: Advanced Distance Vector
* Administrative Distance
    * Internal Routes: 90
    * External Routes: 170
* IP Protocol: 88
* Multicast address: 224.0.0.10
* Default Metric Calculation: Bandwidth and Delay

## Topics

* [Start EIGRP Routing Process](start-eigrp.md)
* [Advertise Networks](network-advertisement.md)
* [Route Summarization](summarization.md)
* [Authentication](authentication.md)
* [Load Balancing](load-balancing.md)
* [EIGRP Stub](eigrp-stub.md)
* [EIGRP Show Commands](eigrp-show-cmds.md)

## Important Terms

* **Reported Distance** (RD) - The metric for a given destination as advertised by your EIGRP neighbor.

* **Feasible Distance** (FD) - The computed metric of a route as perceived by the local router. You take the reported distance from your neighboring router and add the additional distance that your router added.

* **Successor** - The route for a given prefix with the lowest Feasible Distance. This is the route that is installed in the routing table. EIGRP does support equal cost load balancing and you can have multiple successor routes if you have multiple paths to a prefix with the same feasible distance. 

* **Feasibility Condition** - The mechanism EIGRP uses to determine whether a route can be used as a backup path. The feasibility condition compares a neighboring routers reported distance for a prefix against the feasible distance of the successor route. If the reported distance is less than the feasible distance the route is assumed to be a loop free feasible successor route. 

* **Feasible Successor** - A route that meets the feasibilty condition and can be used as a backup path to a given destination.

* **Stuck In Active** (SIA) - When a route is lost from the network an EIGRP router with this destination in their topology table will transition the route to Active. Next the router will query all of it's neighbors to see if they know how to reach the destination and start a Stuck In Active timer. All neighboring routers must reply to your query (whether they have a path or not) before the SIA timer expires. By default this timer is 3 minutes. If a neighbor does not reply before the timer expires the router will say the neighbor is stuck in active and sever the neighborship. Upon doing so it will need to query all remaining neighbors for any destinations it just lost a path to by severing the neighborship.