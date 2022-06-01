## Starting the EIGRP Routing Process

### Classic Mode

To start the EIGRP routing process in classic mode you need to enter the global configuration mode command: router eigrp *autonomous-system-number* The autonomous system number needs to match for all EIGRP routers in the routing domain and is an integer value of 1-65535. 

```
router eigrp 100
```

### Named Mode

To start the EIGRP routing process in named mode you need to enter the global configuration mode command: router eigrp *name* The name is locally significant. After entering this command you will need to enter a second command to truly start the routing process: address-family ipv4 unicast autonomous-system *autonomous-system-number* The autonomous system number needs to match for all EIGRP routers in the routing domain and is an integer value of 1-65535.

**Note** you can shorten the address-family ipv4 unicast autonomous-system *autonomous-system-number* command to *address-family autonomous-system x* and it will default to ipv4 unicast. 

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
````