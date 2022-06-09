## GRE Tunnel Recursive Routing

Recursive routing is an issue that occurs when two routers configured with a GRE tunnel exchange dynamic routing information across the tunnel. The scenario is met when the routers learn and install the route for the configured tunnel destination network in it's routing table.

When the router identifies recursive routing is occurring it temporarily shuts down the tunnel interface. This issue impacts RIP, EIGRP, and OSPF. To prevent the issue from occurring the simplest method is to set the tunnel interface to passive within the routing protocol configuration. 

As an alternative to the passive interface command, you could configure an offset-list and influence the metric in a way that that prevents it from being added to the routing table. 

### Offset-List Example

If you want to prevent recursive routing on a tunnel interface, but don't want to use the routing protocol passive-interface command you can configure an offset-list. In the example below we will tell RIP to increase the hop count on all routes advertised across tunnel1 to 10. 

```
router rip
 offset-list 0 out 10 tunnel 1
```
