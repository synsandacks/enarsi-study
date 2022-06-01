## EIGRP Stub

Configuring an EIGRP router as a Stub is another method to help limit the EIGRP query scope. By configuring a router as a stub it will send queries if it loses an EIGRP learned route, but other routers in the EIGRP domain will not attempt to query a stub.

By default when you configure an eigrp router as a stub with the *eigrp stub* eigrp router sub configuration mode command it defaults to advertising connected and summary routes. You can pick and choose what type of routes you want to advertise or set the stub mode to receive-only. 

```
(config-router)#eigrp stub ?
  connected      Do advertise connected routes
  leak-map       Allow dynamic prefixes based on the leak-map
  receive-only   Set receive only neighbor
  redistributed  Do advertise redistributed routes
  static         Do advertise static routes
  summary        Do advertise summary routes
  <cr>
```

### Classic Mode

To configure a router running EIGRP classic mode as a stub you do so by issuing the *eigrp stub [optional advertisements]* eigrp router sub configuration mode command.

```
router eigrp 100
 eigrp stub receive-only
```

### Named Mode

To configure a router running EIGRP named mode as a stub you do so by issuing the *eigrp stub [optional advertisements]* eigrp router address-family sub configuration mode command.

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
  eigrp stub connected
```