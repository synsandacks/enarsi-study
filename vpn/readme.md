## VPN Technologies

## Topics

* [GRE Tunnel Recursive Routing](gre-recursive.md)

## Important Terms

* **Virtual Routing and Forwarding (VRF)** - The ability to create multiple and logically separated routing instances on a single router. This functionality allows you to assign interfaces to a specific VRF and by default no routing information or reachability will be achieved outside of the specific VRF.

* **Generic Route Encapsulation** - A tunneling protocol that allows you to encapsulate many protocols into a unicast packet to be sent over a point to point link.

* **Multipoint GRE (mGRE)** - A tunnel mode that allows multiple tunnel destinations to be associated with a single tunnel interface.

* **Recursive Routing** - An issue that occurs when two routers configured with a GRE tunnel exchange dynamic routing information across the tunnel. The scenario is met when the routers learn and install the route for the configured tunnel destination network in it's routing table.

* **Non Broadcast Multiple Access (NBMA) Address** - In overlay networks the NMBA address is the IP address associated with the underlay network. This is typically the IP address associated with the tunnel source of a GRE tunnel.

* **Next Hop Resolution Protocol** - A protocol that maps NBMA addresses to their associated private IP address. This allows for the dynamic establishment of GRE tunnels.

* **Dynamic Multipoint VPN (DMVPN)** - Allows for the dynamic establishment of tunnels from branch sites back to the data center or wherever you determine to be your hub site. DMVPN is a combination of mGRE, NHRP, and IPSEC. 