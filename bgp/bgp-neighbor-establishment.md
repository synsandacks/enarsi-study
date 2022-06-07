## BGP Neighbor Establishment

In BGP you have to define an IP address for any neighbors you want to form an adjacency with. This differs from OSPF and EIGRP where multicast is used for neighbor discovery. If you plan to peer with a loopback IP address you will need to define your update-source for your given neighbor. Another important consideration with regard to eBGP peers is multihop. By default BGP anticipates that your peer will be layer 3 adjacent (1 hop away). If your potential neighbor is more than one hop away you will need to configure multihop. When you configure multihop for a peer you are setting the TTL in the IP packet that is being used for session establishment. 

### iBGP

```
router bgp 100
 neighbor 2.2.2.2 remote-as 100
 neighbor 2.2.2.2 update-source loopback 0
```

### eBGP multihop

In the example below we are configuring eBGP for a peer that isn't layer 3 adjacent. With this configuration we are telling BGP that my local routers path to 30.30.30.30 includes one layer 3 hop. If we did not configure multihop, the next hop router would decrement the TTL (default of 1) in the ip packet and since it would now be 0 it would be discarded. By configuring the TTL as 2 the next hop router decrements the TTL and then fowards the packet to 30.30.30.30 for neighbor establishment. 

```
router bgp 100
 neighbor 30.30.30.30 remote-as 300
 neighbor 30.30.30.30 update-source
 neighbor 30.30.30.30 ebgp-multihop 2