## Advertising Networks in BGP

Network advertisments are configured in the *address-family ipv4* router BGP sub configuration mode. Like other routing protocols the network is advertised with the network command. Where BGP differs from OSPF and EIGRP is that the network command and the associated mask are what will be advertised into BGP. Where in OSPF and EIGRP your network statement is telling the router what interfaces should be advertised into the routing domain. This matters because it allows BGP to advertise routes that are in it's routing table, but not directly connected.

### Configuration Example

```
router bgp 100
 address-family ipv4
  network 100.100.100.0 mask 255.255.255.0
```
