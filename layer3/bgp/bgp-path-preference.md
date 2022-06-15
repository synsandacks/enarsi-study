## BGP Path Preference

BGP has many attributes that help decide the best path to a given route. These attributes are in a hierarchical order to determine the preferred path to a network where you have multiple paths to get there.

1. **Weight** - This attribute is specific to Cisco routers and is locally significant. It holds the highest value and routes with the highest configured Weight attribute will always be preferred. 

2. **Local Preference** - This attribute is shared throughout a iBGP routing domain and routes with the highest local preference are preferred. 

3. **Origin-AS** - If a weight and local preference aren't defined a router will prefer paths that it originated. This is denoted by a next hop of 0.0.0.0 within the BGP table. 

4. **AS-Path** - Prefer routes with the shortest AS-PATH.

5. **Origin Type** - Prefer routes based on origin type. Listed below from highest to lowest preference.

    * IGP learned routes
    * EGP learned routes (Likely will never see this as it's specific to the External Gateway Protocol.)
    * INCOMPLETE

6. **Multi-Exit Discriminator (MED)** - Prefer routes with the lowest configured MED or metric. 

7. **eBGP vs iBGP** - Prefer eBGP learned routes over iBGP learned routes.

8. **Best IGP Metric** - Prefer the path through the best IGP neighbor as determined by the IGP route metric.

9. **Oldest Path** - Prefer the path that has been in the routing table the longest.

10. **Router-ID** - Prefer the path advertised by the neighbor with the lowest router-id. 

## Controlling Path Preference - Configuring Local Preference

You can assign local preference to specific routes advertised by a BGP neighbor or all of them.

### Assigning Local Preference to All Routes from a Peer

In the example below I will set a local preference of 500 for routes I am learning from my BGP peer of 30.30.30.30.

```
! Create a route-map
route-map SET-LOCAL-PREF permit 10
 set local-preference 500

! Attach the route-map to incoming routes from neighbor 30.30.30.30
router bgp 100
 address-family ipv4
  neighbor 30.30.30.30 route-map SET-LOCAL-PREF in
```

### Assigning Local Preference to Specific Routes from a Peer

In the example below I will set a local preference of 500 for specific routes learned from my BGP peer of 30.30.30.30. To identify the routes you can use a prefix-list or a standard ACL. I personally like to use prefix-lists.

Router 30.30.30.30 is advertising the following prefixes:

* 31.31.31.0/24
* 32.32.32.0/24
* 35.35.35.0/24
* 36.36.36.0/24

I will configure 35.35.35.0/24 and 36.36.36.0/24 with a local preference of 500 and then permit all other routes in.

```
! Create a prefix-list to match specific routes
ip prefix-list PFX-SET-LOCAL-PREF-500 seq 5 permit 35.35.35.0/24
ip prefix-list PFX-SET-LOCAL-PREF-500 seq 10 permit 36.36.36.0/24

! Create a route-map.
route-map SET-LOCAL-PREF permit 10
 match ip address prefix-list PFX-SET-LOCAL-PREF-500
 set local-preference 500

! Create a second permit statement in the route-map to let other advertised routes in.
route-map SET-LOCAL-PREF permit 20

! Attach the route-map to incoming routes from neighbor 30.30.30.30
router bgp 100
 address-family ipv4
  neighbor 30.30.30.30 route-map SET-LOCAL-PREF in
```