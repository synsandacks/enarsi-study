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