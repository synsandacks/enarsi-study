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
