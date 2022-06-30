## DHCP Server

Configuring a DHCP server on an IOS device is incredibly straightforward. Although this isn't a common practice in enterprise environments, it could be great for a small office or site that is disconnected from the main office. Additionally, it might be something you can leverage in an outage situation say if the central DHCP server is down. 

## Steps To Configure 

* Configure DHCP Database Agent - A database agent allows you to store DHCP binding information at a remote location or even to local flash. This is optional configuration, but highly recommended. 

* Define a DHCP Pool - This is where you will configure the network for the DHCP range, assign a default router, dns server, etc. 

* Configure Excluded Addresses - Define any addresses that should be excluded from the DHCP scope. This is another optional component, but at the very least you should exclude the address of the assigned default router in the pool.

## Configure DHCP Database Agent

THe DHCP database agent can be configured to a remote location (recommended) however, if this isn't an option this can be stored locally on the device flash. In the example below I will configure this locally.

```
ip dhcp database flash:/dhcpdatabase
```

Below is the contents of the DHCP database file from a DHCP server that assigned two leases.

```
LABROUTER#more flash:dhcpdatabase
*time* Jun 30 2022 03:09 AM
*version* 5
!IP address     Type  Hardware address   Lease expiration       VRF
192.168.12.2    id    0063.6973.636f.2d35.3030.302e.3030.3032.2e30.3030.302d.4769.302f.30  Jul 01 2022 02:40 AM
192.168.23.1    id    0063.6973.636f.2d35.3030.302e.3030.3033.2e30.3030.302d.4769.302f.30  Jul 01 2022 01:33 AM

!IP address      Type     Hardware address  Interface-name   VRF


!IP address     Interface-name  Lease expiration      Server IP address  Hardware address  Vrf
*end*
```

## Configure DHCP Pool

As previously stated this is the core component of the DHCP configuration. This is where you assign the network for the DHCP pool, along with other key information like default router, dns, etc. 

In the configuration below I will create a pool for 192.168.20.0/24 and configure the following options:

* DNS Server - 1.1.1.1
* Default Router (Default Gateway) - 192.168.20.1
* Local Domain Name - labexample.net
* Lease Duration - 12 Hours

```
ip dhcp pool LABDHCPPOOL
 network 192.168.20.0 255.255.255.0
 dns-server 1.1.1.1
 default-router 192.168.20.1
 domain-name labexample.net
 lease 0 12
```

## Configure Excluded Addresses

At the very least you should exclude any known statically assigned addresses. The default gateway address is a prime candidate for exclusion. It is strongly recommended that you exclude at least a few additional addresses to be used for static assignment at a later date. 

In the example below I will exclude the first 10 useable addresses in the 192.168.20.0/24 network.

```
ip dhcp excluded-address 192.168.20.1 192.168.20.10
```

## Troubleshooting DHCP Server

Even with all of the configuration in place above someone could disable the DHCP server service. There are two ways to confirm that the DHCP Server has been disabled on the router.

### Option 1

Validate what ports the router is listening on by issuing the `show udp` command. By default the DHCP server service listens on port 67. If the device is not listening on port 67 the DHCP service was disabled with a global configuration command. 

Example With DHCP Service Disabled
```
LABROUTER#show udp
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF

```

Example With DHCP Service Running
```
LABROUTERr#show udp
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17     0.0.0.0             0 --any--            67   0   0 1002211   0
```

## Option 2

Validate whether the service is disabled in the running config by issuing the following command:

```
LABROUTER#show running-config | in no service dhcp
no service dhcp
```

If this is in the running config you can enable the DHCP server by issuing the global configuration command `service dhcp`