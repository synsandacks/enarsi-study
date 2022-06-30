## DHCP Relay

DHCP Relay allows a router to relay a DHCP request for a client to the configured DHCP server. The configuration is applied to the interface facing the client.

As an example, lets say interface gi0/1 is facing a client network and acting as a default gateway. The interface is configured with an IP address of 192.168.20.1 and we configured DHCP Relay to the DHCP server at 192.168.10.67. 

```
interface gi0/1
 ip helper-address 192.168.10.67
```

When the router sees the DHCP discover packet broadcasted on the network attached to interface gi0/1, it will forward the DHCP discover in a unicast packet to the configured DHCP server. 

The DHCP server then responds with a DHCP offer again sending unicast traffic back to the router. Once the router receives the DHCP offer it sends a broadcast packet out the interface it received the Discover on. 

Upon receiving the offer, the DHCP client device will need to formally request the information provided in the offer. The DHCP Client device now broadcasts a DHCP request packet with the provided IP information. 

The router receives this broadcast and again sends a unicast DHCP request packet back to the DHCP server. The DHCP server then replies with a DHCP Acknowledgment sent unicast back to the Router. After receiving the DHCP Acknowledgment the router sends the packet as a broadcast out the interface the original DHCP discover was received on. 