## EIGRP Authentication

To configure authentication in EIGRP you first need to create a key chain and at least one key. This applies to both classic and named mode of EIGRP. 

```
key chain MyKeyChain
 key 1
  key-string EIGRPAuthPassword
```

The key chain is what is referenced when configurations authentication and you can have multiple keys configured. In addition to the basic command you can configure send and accept lifetimes that match a specific date and time that a given key is valid. 

### Classic Mode

To enable authentication in EIGRP classic mode you do so on the interfaces where EIGRP neighborships are formed. As soon as you enable authentication the neighborship will go down until you configure authentication on the neighboring router. 

```
interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5
 ip authentication key-chain eigrp 100 MyKeyChain
```

### Named Mode

To enable authentication in EIGRP named mode you do so on the af-interface sub configuration mode within the router eigrp configuration mode. You can configure this on the af-interfaces where EIGRP neighborships are formed or enable it on all interfaces by issuing the commands on the af-interface default. As soon as you enable authentication the neighborship will go down until you configure authentication on the neighboring router. 

```
router eigrp NamedMode
 address-family ipv4 unicast autonomous-system 100
  af-interface GigabitEthernet0/0
   authentication mode md5
   authentication key-chain MyKeyChain
  exit-af-interface
```