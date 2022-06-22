## IP Access Lists

IP access control lists (ACL) are primarily used to control what traffic can ingress or egress a network device. In addition to this they can be used to control routing updates, route redistribution, and limiting debug output. These notes will primarily focus on the basics of ACLs controlling ingress and egress traffic. 

Two primary types of Access Lists exist, Standard and Extended. 

* Standard ACLs only match on the IPV4 source address. 

* Extended ACLs allow you to match on IPV4 source and destination addresses, IP protocols, and layer 4 source and destination ports. (TCP/UDP)

Both types of ACLs can be created either using a numbered list or name. The following numbering conventions apply.

```
LABROUTER(config)#ip access-list standard ?
  <1-99>       Standard IP access-list number
  <1300-1999>  Standard IP access-list number (expanded range)
  WORD         Access-list name

LABROUTER(config)#ip access-list extended ?
  <100-199>    Extended IP access-list number
  <2000-2699>  Extended IP access-list number (expanded range)
  WORD         Access-list name
```

When creating ACLs I prefer to used named ACLs because it allows me to give the ACL a meaningful name to readily identify what the ACL is doing. In addition to this simple benefit, there are another of other benefits with the biggest being that you can remove individual permit or deny statements. With numbered ACLs removing any ACE will remove the entire ACL. 

### ACL Placement

When applying ACLs on a router the rule is one IPV4 ACL per direction per interface. This means on a router with only gi0/0 and gi0/1 you could technically have four ACLs configured. 

It's recommended that extended ACLs be applied as close to the source traffic as possible. In doing so you reduce unnecessary network traffic. Alternatively standard ACLs should be applied as close to the destination as possible. Since standard ACLs match on the IPV4 source address only you want to it closer to the destination so you don't inadvertently deny more traffic than you had planned. 

One extra thing to note is ACLs do not apply to traffic that is generated on the router itself. As an example if you applied an ACL that blocked all outbound IPV4 traffic sourced from 192.168.0.0/24 to 10.0.0.0/24. If the router had an IP address configured in the 192.168.0.0/24 range and you attempted to initiate any traffic to 10.0.0/24 from that interface the ACL wouldn't even be considered. This is highlighted because it may confuse you during the troubleshooting process. 

### Implicit Deny

All ACLs have an implicit deny at the end of the ACL which means any traffic that isn't explicitly permitted will be denied. As a good rule of thumb you should add a `deny ip any any` to any ACL so that the implicit deny is explicitly defined to avoid confusion. 

### Standard ACL Configuration Example

When creating a standard ACL you have to consider two parts the network prefix that is in scope and the associated wild card mask. So as an example say we have a web server running on LABROUTER and we want to restrict traffic to this web server by only permitting traffic sourced from 192.168.20.0/24 inbound.

```
! Create the standard / named ACL.
ip access-list standard ACL-LABEXAMPLE-STD
 permit 192.168.20.0 0.0.0.255
 deny any

! Apply the ACL to a network interface. 
interface GigabitEthernet0/0
 ip access-group ACL-LABEXAMPLE-STD in
end
```
 
### Extended ACL Configuration Example

Following a similar example as above say you want to restrict the host 192.168.30.14 from accessing an https web server at 10.0.0.22. You would want to restrict this traffic on the router closest to the source. 

```
! Create the extended / Named ACL
ip access-list extended ACL-LABEXAMPLE-EXTD
 permit tcp host 192.168.30.14 host 10.0.0.22 eq 443
 deny   ip any any

 ! Apply the ACL to a network interface.
interface GigabitEthernet0/0
 ip access-group ACL-LABEXAMPLE-EXTD out.
 ```


