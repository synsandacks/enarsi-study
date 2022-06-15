## IP Prefix Lists

IP prefix lists are a list type that you can use to match on prefixes either in or destined to your local routing table. In other words they can be leveraged to control route advertisement from your router or the ingestion of routes advertised to you from another router. Like access control lists prefix lists end with an implicit deny statement, so if you are denying routes with a prefix list don't forget to end with a permit any statement.

To create a prefix list you do so with the following global configuration mode command:

```
ip prefix-list PFX-DENY-LIST-EXAMPLE seq 10 deny 100.100.100.25/32
ip prefix-list PFX-DENY-LIST-EXAMPLE seq 20 permit 0.0.0.0/0 le 32
```

Now lets break this example down. First `PFX-DENY-LIST-EXAMPLE` is the name of the prefix list. Next I created a deny statement for a very specific /32 route. A match of this prefix would only occur if 100.100.100.25/32 existed in my routing table or if I'm using the prefix list to filter inbound routes, if that specific prefix was advertised to me. Next I created a permit sequence that matches any other prefix, including a default route. 

The above example introduced the LE option and there is another option as well GE. LE is less than or equal to and GE is greater than or equal to. By adding these additional matching rules we can get very specific on what we match on. 

Lets say we have 10.100.0.0/24 subnetted in the following manner:

* 10.100.0.0/25
* 10.100.0.128/26
* 10.100.0.192/27
* 10.100.0.224/28
* 10.100.0.240/29
* 10.100.0.248/30
* 10.100.0.252/30

We could create a prefix list that denies any of the subnets that are a /27, but less than a /30. 

```
ip prefix-list PFX-DENY-LIST-EXAMPLE seq 10 deny 10.100.0.192/26 le 29
ip prefix-list PFX-DENY-LIST-EXAMPLE seq 20 permit 0.0.0.0/0 le 32
```

The deny entry is using the following logic:

Deny any network that matches 10.10.0.192 but only match on the first 26 bits. After that match any network that follows that initial logic and has subnet mask less than or equal to 29. 

A prefix list alone doesn't result in any filtering. You use the prefix list as a tool to help match ip addresses either in a route-map or a distribute list. 

Lets say we want to use the above prefix list as a tool to filter routes being advertised out to our EIGRP neighbor attached to GI0/0. This can be achieved by using a distribute list within EIGRP.

```
router eigrp 100
 distribute-list prefix PFX-DENY-LIST-EXAMPLE out gigabitEthernet 0/0
```

After applying the above config EIGRP would graceful restart and then the routes we filtered would no longer be in our peers routing table.