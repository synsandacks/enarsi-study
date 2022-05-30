## EIGRP Quick Info

* Routing Protocol Type: Advanced Distance Vector
* Administrative Distance
    * Internal Routes: 90
    * External Routes: 170
* IP Protocol: 88
* Multicast address: 224.0.0.10
* Default Metric Calculation: Bandwidth and Delay

## Starting the EIGRP Routing Process

### Classic Mode

To start the EIGRP routing process in classic mode you need to enter the global configuration mode command: router eigrp *autonomous-system-number* The autonomous system number needs to match for all EIGRP routers in the routing domain and is an integer value of 1-65535. 

```
router eigrp 100
```

### Named Mode

To start the EIGRP routing process in named mode you need to enter the global configuration mode command: router eigrp *name* The name is locally significant. After entering this command you will need to enter a second command to truly start the routing process: address-family ipv4 unicast autonomous-system *autonomous-system-number* The autonomous system number needs to match for all EIGRP routers in the routing domain and is an integer value of 1-65535. 

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
````

## Network Advertisement

In EIGRP you don't necessarily advertise networks, but you tell the routing process which networks to enable the routing process on. By default all interfaces that fall in scope of the prefix associated with the network command will participate in EIGRP. 

### Classic Mode

To enable an interface to participate in EIGRP in classic mode you enter the network *x.x.x.x* command in the EIGRP router sub configuration mode. This will add the network at it's classful boundry. To enable a specific prefix use network *x.x.x.x* *y.y.y.y* where y.y.y.y is the wildcard mask associated with your prefix. To enable all interfaces at once you could enter network 0.0.0.0.

```
router eigrp 100
 network 10.1.1.0 0.0.0.255
```

### Named Mode

To enable an interface to participate in EIGRP in named mode you enter the network *x.x.x.x* command in the EIGRP router address-family ipv4 unicast autonomous-system *x* sub configuration mode. This will add the network at it's classful boundry. To enable a specific prefix use network *x.x.x.x* *y.y.y.y* where y.y.y.y is the wildcard mask associated with your prefix. To enable all interfaces at once you could enter network 0.0.0.0.

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
 network 10.1.0.0 0.0.0.255
```

## Route Summarization

Route summarization helps limit query scope for a prefix covered by the summary route in the EIGRP routing domain. As an example lets say you have the following prefixes advertised via EIGRP:

* 10.1.1.0/26
* 10.1.1.64/26
* 10.1.1.128/26
* 10.1.1.192/26

If any one of these networks go down every router that had the prefix in it's routing table would query to see if any other routers have a path to that network. Summarization helps limit the scope of these queries. If you add a summary route of 10.1.1.0/24 on the interfaces where you have EIGRP neighborships, you limit the query scope to 1 router querying its neighbors. The neighbors will reply that they don't have a path to that network because the specific prefix was never in it's routing table and since they didn't lose any routes they won't query any of their neighbors.

When a summary route is created it will show up in the local routers routing table as an EIGRP route with a next hop of NULL0. 

### Classic Mode

To configure an EIGRP summary route in classic mode drop into the interface configuration mode on the interface where you have an EIGRP neighbor and issue: ip summary-address eigrp *asn* *network* *subnet mask*.

```
interface GigabitEthernet0/0
 ip summary-address eigrp 100 10.1.1.0 255.255.255.0
```
### Named Mode

To configure an EIGRP summary route in named mode you configure it on the af-interface sub configuration mode within the EIGRP router address-family ipv4 unicast autonomous-system *x* sub configuration mode. Once you're in teh appropriate af-interface you add the summary with: summary-address *network* *subnet mask*.

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
  af-interface gigabitEthernet 0/0
   summary-address 10.1.1.0 255.255.255.0
```

## EIGRP Authentication

To configure authentication in EIGRP you first need to create a key chain and at least one key. This applies to both classic and named mode of EIGRP. 

```
key chain MyKeyChain
 key 1
  key-string EIGRPAuthPassword
```

The key chain is what is referenced when configurating authentication and you can have multiple keys configured. In addition to the basic command you can configure send and accept lifetimes that match a specific date and time that a given key is valid. 

### Classic Mode

To enable authentication in EIGRP classic mode you do so on the interfaces where EIGRP neighborships are formed. As soon as you enable authentcation the neighborship will go down until you configure authentication on the neighboring router. 

```
interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5
 ip authentication key-chain eigrp 100 MyKeyChain
```

### Named Mode

To enable authentication in EIGRP named mode you do so on the af-interface sub configuration mode within the router eigrp configuration mode. You can configure this on the af-interfaces where EIGRP neighborships are formed or enable it on all interfaces by issuing the commands on the af-interface default. As soon as you enable authentcation the neighborship will go down until you configure authentication on the neighboring router. 

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
  af-interface GigabitEthernet0/0
   authentication mode md5
   authentication key-chain MyKeyChain
  exit-af-interface
```
