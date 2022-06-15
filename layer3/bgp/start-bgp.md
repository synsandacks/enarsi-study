## Starting the BGP Routing Process

To start the BGP routing process you enter the global configuration mode command: router bgp *autonomous-system-number* 

If you are configuring iBGP the autonomous system number (ASN) will match for all iBGP peers. If you are configuring eBGP your local ASN would be different than your neighboring router.

The ASN is a 4-byte value which has a range of 0 - 4294967295. The following reservations exist for usage:

* Public ASN range: 1 - 64495
* Private ASN range: 64512 - 65534
* Public ASN range: 131072 - 4199999999 
* Private ASN Range: 4200000000 - 4294967294

### Configuration Example 

```
router bgp 64512
```
