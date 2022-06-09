## BGP Quick Info

* Routing Protocol Type:  Path Vector
* Administrative Distance
    * Internal Routes (iBGP): 200
    * External Routes (eBGP): 20
* TCP Port: 179
* Path Selection: Controlled by various attributes.s

## Topics

* [Start BGP Routing Process](start-bgp.md)
* [BGP Neighbor Establishment](bgp-neighbor-establishment.md)
* [Advertise Networks](bgp-network-advertisement.md)
* [Controlling Path Preference](bgp-path-preference.md)

## Important Terms

* **Autonomous System Number** - A unique identifier assigned to a company or organization to be used as the identifier within BGP. The ASN is a 4-byte value that has a range of 0 - 4294967295. Publicly allocated ASNs are assigned by regional internet registries (RIR). In the United States this would be ARIN.

    * Public ASN range: 1 - 64495
    * Private ASN range: 64512 - 65534
    * Public ASN range: 131072 - 4199999999 
    * Private ASN Range: 4200000000 - 4294967294

* **iBGP** - BGP peering where the neighbors share a common ASN.

* **eBGP** - BGP peering where the neighbors do not share a common ASN.

* **Network Layer Reachability (NLRI)** - The data contained in BGP Update packets providing peers with the IP prefixes and associated subnet masks in CIDR notation.