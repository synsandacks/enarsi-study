## VRF-Lite

VRF-Lite allows you to have multiple virtual routing tables on a single router. This functionality allows you to assign interfaces to a specific VRF and by default no routing information or reachability will be achieved outside of the specific VRF. This is a common method used to support overlay/underlay networks and within ISPs to create logically separate environments for customers. 

### Configuration Example

To configure VRF-Lite you start by creating the vrf in global configuration mode.

```
ip vrf VRFEXAMPLE
```

Next assign a given interface to the VRF. An interface can only be assigned to a single VRF and upon moving an interface to a VRF it will lose any configured IP addressing. 

```
interface loopback0
 ip vrf forwarding VRFEXAMPLE
```

This flexibility extends beyond interface configuration. I won't go into details here, but you can configure routing protocols to participate in specific VRF's or on a shared router you could have multiple instances of a routing protocol participating in specific VRFs. 

### Show Commands

Validate all VRF's configured on a given device. This command shows all configured VRFs and associated interfaces. 

```
show vrf
```

Look at a specific VRF routing table

```
show ip route vrf VRFEXAMPLE
```

Execute a ping command to validate reachability within a vrf

```
ping vrf VRFEXAMPLE 192.168.12.2
```