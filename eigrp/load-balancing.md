## Load Balancing

By default, EIGRP will perform equal cost load balancing on up to 4 links to a given network. However, in EIGRP you can configure it to support unequal cost load balancing as well. This is done with the *variance n* command where *n* is the acceptable amount of variance between the route metrics. With a variance of 2 EIGRP would load balance traffic across the successor route and the feasible successor route where the metric of the feasible succesor is less than 2 times the metric of the successor route. 

As an example, let’s assume Router1 has two paths to the network 10.1.1.0/24. The successor route has a metric of 20 and the feasible successor has a metric of 40. With a variance of 3 EIGRP would load balance the traffic across both links to this network.

By default, EIGRP will distribute the traffic unevenly across the two links using a ratio of the metrics. Given the example above say the successor route has a metric of 20 and the feasible successor route has a metric of 40. The router will load balancing the traffic with an approximate ratio of 2:1 to the successor. This can be confirmed by performing the following math:

The worst metric is the starting point, so 40. Now divide this number by the metric of each path and round down to the nearest whole number.

Successor route: 40/20 = 2
Feasible Successor Route: 40/40 = 1
Ratio: 2(successor):1(feasible successor)

As another example let’s say the successor route had a metric of 25 and the feasible successor has a metric of 40.

Successor route: 40/25 = 1 (rounding down to nearest whole number)
Feasible Successor Route: 40/40 = 1
Ratio: 1(successor):1(feasible successor)

### Classic Mode

To enable unequal cost load balancing in EIGRP classic mode, enter the *variance n* command in the eigrp router configuration mode command.

```
router eigrp 100
 variance 3
```

### Named Mode

To enable unequal cost load balancing in EIGRP named mode, enter the *variance n* command in the topology base sub configuration mode within the ipv4 unicast address family associated with your ASN. 

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
  topology base
   variance 3
  exit-af-topology
```