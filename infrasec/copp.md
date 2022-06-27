## Control Plane Policing (CoPP)

Control Plane Policing is a means of protecting a network device from a denial of service scenario. Certain types of traffic are punted up to the route processor (CPU) for additional processing. This could be management traffic like ssh or telnet, routing processes, or any IP traffic destined to an IP configured on the device. To protect the CPU from being overwhelmed either from malicious users or misconfiguration you can apply QoS to any traffic destined to the control plane. 

To configure CoPP the following must be completed:

* Create an ACL to match source traffic (Only required if you want to police based on source IP)
* Create a class-map to match on specific IPs or applications. 
* Create a policy-map
    * A policy map can include multiple class-maps and apply unique policy to each
* Attach the policy-map to the control-plane.
    * input applies to traffic being punted to the control plane.
    * output applies to traffic being injected into the forwarding plane.

In the example below I will create two class-maps and apply them to control-plane input.

* Deny SSH traffic from 192.168.10.1
* Limit all ICMP traffic to 100 Packets Per Second 

## Create ACLs

```
ip access-list extended ACL-RESTRICT-SSH
 permit tcp host 192.168.10.1 any eq 22
!
ip access-list extended ACL-MATCH-ICMP
 permit icmp any any
```

## Create class-maps

In my labbing I ran into issues matching layer 3 and up protocols in the class map by leveraging the class-map *match protocol x* command.

```
class-map CLASS-CP-SSH
 match access-group ACL-RESTRICT-SSH
!
class-map CLASS-CP-ICMP
 match access-group ACL-MATCH-ICMP
```

## Create policy-map

It's important to know that any traffic that isn't explicitly matched via the ACL associated with the CLASS-CP-SSH class map will be permitted. Effectively the way the ACL is written I am permitting traffic to be dropped by the policy map. 

```
policy-map POLICY-CONTROL-PLANE
 class CLASS-CP-SSH
  drop
  !
 class CLASS-CP-ICMP
  police rate 100 pps
```

## Attach Policy to Control Plane

```
control-plane
 service policy input POLICY-CONTROL-PLANE
```