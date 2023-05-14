
### Network Route vs Host Route

Network Route = Connected Route = A route to a nw/subnet (mask length < 32)
Host Route = Local Route = A route to a specific host (/32 mask)

# Dynamic Routing

- Routes can use dynamic Routing protocols to advertise info about the routes they know to other Routers
- They form *adjacencies* (aka  *neighbor relationships* OR *neighborships*) with adjacent routes to exchange this info


If multiple routes to a destination are learned, the R determins which route is superior and adds it to the R table.
- it uses *metric* of the route to decide which is superior
- lower metric = superior

## Types

1. IGP
2. EGP

![[Pasted image 20230510220256.png]]


![[Pasted image 20230510220342.png]]


### Distance Vector  Routing Protocols

- invented before Link state protocols
- Early examples are *RIPv1* and Cisco's proprietary protoc *IGRP* (predecessor to *EIGRP*)
- Distance Vector protos operate by sending the following to their directly connected neighbors
	- their known destination nw
	- their metric to reach their known destination networks
- this method of route info sharing is aka **Routing by Rumor**
	- This is because the R does not know abt the nw beyond its neighbors.
	- It only knows the info that its neighbor tells it
- Called "Distance Vector" coz the Rs only learn the *distance* (metric) and *vector* (direction, the next-hop R) of each route

### Link State Routing Protocols

- When using *link state*, every R creates a *connectivity map* of the nw
	- This map will be the same on each router
- To allow this, each R advertises info abt its interfaces (connected nw) to its neighbors
	- These adverts are pased along to other Rs, until all Rs in the nw develop the same map of the nw
- Each R independently uses this map to calculate the best routes to each destination

#### Pros and Cons vs Distance Vector
- Link State protos use more resources (CPU) on the R, coz more info is shared
- However, Link State protos tend to be faster in reacting to changes in the nw than DV protos

## Dynamic Routing Protocols Metrics

- A Rs route table contains the best route to each destination nw it knows abt
- If a R using a dynamic R proto learns 2 different Routes to the *same destination* (from the same protocol), how does it determines which is 'best'?

It uses *metric* value of the routes to determine which is best

lower metric = better


**Note**: *same destination* means same nw, subnet mask

**Note**: Static Routes have a metric of 0 by default and AD of 1

**Note**: if *metric* is same for 2 routes from same protocol to same destination, the R stores both routes in the R table and *load balances* bw them
- This is called **ECMP** (Equal Cost Multi Path)


**Note**: metric is used to compare routes given by same protocol, WHEREAS, *Administrative Distance* is used to compare Routes by diff protos 


Blue is AD
Red is Metric
![[Pasted image 20230510221728.png]]

![[Pasted image 20230510221904.png]]


## Administrative Distance

- AD is used to compare a route learned via two different protocols
	- companies usually use 1 protos
	- however, they may use 2 different protos when e.g. two different companies connect their nw to share info

A lower AD is preferred
- and indicates that the R proto is considered more "trustworthy" (more likely to select good routes)

![[Pasted image 20230510222231.png]]

Unusable Route - The R does not believe the source of that route and does not install the route in the R Table

**NOTE**: AD can be changed

**NOTE**: AD of a static route can be changed by the following command
`ip route <ip_addr> <subnet_mask> <next-hop> <AD>`
where AD is from 1-255


### Floating Static Routes

By changing the AD of a static Route, we can make it less preferred than routes learned by a dynamic proto to the same destination (make sure the AD is higher than the Routing protos one)

This is called a Floating static routes

The route will be inactive (not in the R table) unless the route learned by the Dynamic proto is removed (ex. the remote router stops advertising it for some reason, or interface failure)



# Quiz

1. The following routes to destination 10.1.1.0/24 are learned
	- next hop 192.168.1.1, learned via RIP, metric 5
	- next hop 192.168.2.1, learned via RIP, metric 3
	- next hop, 192.168.3.1, learned via OSPF, metric 10

Which route to 10.1.1.0/24 will be added to the route table?

Answer: OSPF
- since destination is same, and different protos, AD is used to compare
- OSPF has lower AD than RIP


2. R1 learns 4 routes to 192.168.1.0/24 through multiple routing protos: RIP, EIGRP, OSPF and IS-IS. Which routes it stores

Answer: EIGRP
- lowest AD


3. R1 learns 2 routes to 172.16.0.0/16 via RIP, 1 via 10.0.0.1 and other via 10.1.0.1. Both routes are 5 hops away, which route will go into R table?

Answer: Both
- and load balanced



![[Pasted image 20230510223123.png]]
