## EIGRP Show Commands

* `show run | section router eigrp` - Show EIGRP router configuration.

* `show ip protocols` - Detailed information about the current running EIGRP configuration.

* `show eigrp address-family ipv4 neighbors` - See all EIGRP neighbors.

* `show eigrp address-family ipv4 topology` - See all routes that you've learned via EIGRP and their current status. P = passive and this is the preferred state indicating that a path to a network has been identified. 

* `show eigrp address-family ipv4 secondary-paths` - See all routes and their backup paths to get to a given destination. 

* `show eigrp address-family ipv4 active` - Show all routes that currently active (searching for a path). 