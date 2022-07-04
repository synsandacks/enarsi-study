## EIGRP for IPv6

Like EIGRP for IPv4 there are two ways to configure EIGRP for IPv6, Legacy Mode and Named Mode using address families. The latter is the preferred method. There is one key difference between both modes that should be noted. By default named mode will advertise all IPv6 configured interfaces that are in an UP/UP state. When using legacy mode you have to manually enable the interface to participate in EIGRP by executing the interface sub-configuration mode command *`ipv6 eigrp ASN`* where ASN is the autonomous system number associated with your EIGRP routing process. 

Another important note for both modes of EIGRP for IPv6 is if the router does not have any interfaces configured for IPv4 the EIGRP for IPv6 routing process won't be able to initialize. This is because every EIGRP router needs a configured router-id. This router-id is only in IPv4 notation and automatically selected by leveraging configured IPv4 interfaces. [(Full Explanation Here)](eigrp-rid.md)

One final note before we start configuring EIGRP for IPv6 is that you must enable IPv6 unicast routing on Cisco routers or switches before a device can participate in any IPv6 routing functions. This is achieved by executing the global configuration command *`ipv6 unicast-routing`*

## Legacy Mode

### Enable IPv6 Unicast Routing

Enable IPv6 Unicast Routing by executing the following global configuration mode command.

```
ipv6 unicast-routing
```

### Start EIGRP IPv6 Routing process

Start the EIGRP IPv6 legacy mode routing process and configure the router-id.

```
ipv6 router eigrp 6
 eigrp router-id 1.1.1.1
```

### Enable Interface To Participate in EIGRP

In IPv6 EIGRP legacy mode you must enable EIGRP on the interfaces directly. 

```
interface GigabitEthernet0/0
 ipv6 eigrp 6
```

### Legacy Show Commands

```
LABROUTER#show ipv6 eigrp ?
  <1-65535>       Autonomous System
  events          Events logged
  interfaces      interfaces
  neighbors       Neighbors
  sia-statistics  SIA Statistics
  timers          Timers
  topology        Select Topology
  traffic         Traffic Statistics
```

## Named Mode

### Enable IPv6 Unicast Routing

Enable IPv6 Unicast Routing by executing the following global configuration mode command.

```
ipv6 unicast-routing
```
### Start EIGRP IPv6 Routing process

Start the EIGRP IPv6 named mode and enter IPV6 address family to start routing process and configure the router-id.

```
router eigrp LAB
 address-family ipv6 autonomous-system 6
  eigrp router-id 1.1.1.1
```

### Prevent the advertisement of a connected network

In named mode using address families all IPv6 configured interfaces in UP/UP status participate in the routing process by default. The commands below will demonstrate how to prevent Loopback0 from participating in EIGRP.

```
router eigrp LAB
 address-family ipv6 autonomous-system 6
  af-interface loopback0
   shutdown
```

### Named Mode Show Commands

```
LABROUTER#show eigrp address-family ipv6 ?
  <1-65535>   Autonomous System
  accounting  Prefix Accounting
  events      Events logged
  interfaces  interfaces
  neighbors   Neighbors
  perf-stats  Performance statistics
  timers      Timers
  topology    Select Topology
  traffic     Traffic Statistics
  vrf         Select a VPN Routing/Forwarding instance
```