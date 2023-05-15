
Stands for *Open Shortest Path First*

Uses the *Shortest Path First* algo (aka Djikstra's algo)


### Versions


OSPF v1
- 1989
- Old, not used anymore

OSPF v2
- 1998
- Used for IPv4

OSPF v3
- 2008
- Used for IPv6 
- (can also be used for IPv4, but usually v2 is used)


### LSA & LSDB

Rs store info about the nw in *LSAs* (Link State Advertisements), which are organized in a structure called *LSDB* (Link State DB)

Rs will *flood* LSAs until all Rs in OSPF *area* develop the same map of the nw (LSDB)

Each LSA has an aging timer (30 min by default).

The LSA will be flooded again after the timer expires


![[Pasted image 20230513213251.png]]


There are 3 main steps in the processing of sharing LSAs and determining the best route to each destination in the nw

1. *Become neighbors* with other Rs connected to the same segment
2. *Exchange LSAs* with neighbor Rs
3. *Calculate the best routes* to each destination, and insert them into the R table


### OSPF Areas

OSPF used *areas* to divide up the nw

Small networks can be single-area without any negative effects on performance
- However, for larger networks (e.g. 500 routes with hunders of subnets), using multiple areas is recommended

In larger nws, a single-area design can have negative effects:
- the SPF algo takes more time to calculate routes
- the SPF algo requires exponentially more processing power on the Rs
- the larger LSDB takes up more memory on the Rs
- any small change in the nw causes every R to flood LSAs and run the SPF algo again

By diving a large OSPF nw into several smaller areas, you can avoid the above negative effects.

![[Pasted image 20230513214153.png]]

vs

![[Pasted image 20230513214204.png]]


#### Area Rules

- OSPF areas should be *contiguous*
	- non-contiguous is not allowed in OSPF and will cause problems

Contiguous
![[Pasted image 20230513221357.png]]

vs Non-contiguous
![[Pasted image 20230513221447.png]]

---

- All OSPF areas must have at least 1 ABR connected to the backbone area

Correct
![[Pasted image 20230513221551.png]]

vs Incorrect
![[Pasted image 20230513221603.png]]

---

- OSPF interfaces in the same subnet must be in the same area

Incorrect
![[Pasted image 20230513221716.png]]

vs Correct
![[Pasted image 20230513221736.png]]



### Terminology

#### Area
- Set of Rs and links that share the same LSDB

#### Backbone Area (area 0)
- Is an area that all other areas must connect to

(good)
![[Pasted image 20230513214510.png]]

vs (bad)

![[Pasted image 20230513214518.png]]


#### Internal Routers
- Rs with all interfaces in the same area are called Internal Routers


![[Pasted image 20230513214558.png]]


#### Area Border Routers (ABRs)

Rs with interfaces in multiple areas are called ABRs

ABRs maintain a separate LSDB for each area they are connected to.

**NOTE**: It is recommended that you connect an ABR to a maximum of 2 areas.
- Connecting ABRs to 3+ areas can overburder the R


![[Pasted image 20230513220930.png]]


#### Backbone Routers
- Routers connected to the backbone area (area 0)
- includes ABRs

![[Pasted image 20230513221126.png]]


#### Intra-area Route
- Route to a destination inside the same OSPF area

![[Pasted image 20230513221209.png]]


#### Inter-area Route
- Route to a destination in a different OSPF area

![[Pasted image 20230513221257.png]]


#### Autonomous system boundary Router (ASBR)
- An ASBR is an OSPF router that connects OSPF nw to an external nw



### Cost

OSPF's metric is called *cost*

It is automatically calculated based on the bandwidth (speed) of the interface

It is calculated by

> *reference bandwidth* / interface's bandwidth

where *reference bandwidth* = 100 Mbps by default

Cost cannot be lower than *1* and any smaller values are automatically converted to 1

Examples:
- Reference = 100 mbps / Inteface = 10 mbps = cost of *10*
- Reference = 100 mbps / Interface = 100 mbs = cost of *1*
- Reference = 100 mbps / Interface 1000 mbps = cost of *1*
- Reference = 100 mbps / Interface 100000 mbps = cost of *1*

The default cost of 1 for any connection higher than 100 mbps is not ideal, we can change the reference cost by the following command:

`R1(config-router)# auto-cost reference-bandwidth <megabits_per_second>`

Ensure that Reference bandwidth is consistent across all OSPF Routers in th nw


![[Pasted image 20230514134153.png]]

![[Pasted image 20230514134317.png]]


To check cost of a specific interface, use `show ip ospf interface <interface_id>`

![[Pasted image 20230514134024.png]]


To manually configure a cost of an interface, there are 3 methods:

1. use the command:
	- `R1(config-if)# ip ospf cost <1-65535>`
2. change interface's bandwidth
	- `bandwidth <bandwidth>`
		- **NOTE**: Interface bandwidth is not the same as speed of the interface
		- E.g. bandwidth is just a number, which can be changed, the interface will still operate at its original speed
	- Recommended *not* to use this method
![[Pasted image 20230514134737.png]]


Thus 3 ways to modify OSPF cost
1. change reference bandwidth
2. manually configure cost for an interface
3. change the interface bandwidth


### OSPF neighbors

Making sure that Rs succesfully become OSPF neighbors is the main task in configuring and troubleshooting OSPF

Once Rs become neighbors, they automatically do the work of sharing nw info, calculating routes etc.

#### Hello messages

When OSPF is activated on an interface, the R starts sending OSPF **hello** messages out of the interface at regular intervals (determined by the **hello timer**). 
- These are used to introduce the R to potential OSPF neighbors

The default **hello timer** is *10 seconds* on an Ethernet connection

OSPF hello messages are multicast to `224.0.0.5` (multicast address for all OSPF routers)

OSPF messages are encapsulated in an IP header, with a value of `89` in the *Protocol* field


#### Steps & States

Can use mnemonic `DoITwEELFully` to remember states

![[Pasted image 20230514135706.png]]

![[Pasted image 20230514135740.png]]

![[Pasted image 20230514135807.png]]

![[Pasted image 20230514135840.png]]

![[Pasted image 20230514140209.png]]

![[Pasted image 20230514140234.png]]

![[Pasted image 20230514140255.png]]

![[Pasted image 20230514140346.png]]


##### Summary

![[Pasted image 20230514140440.png]]


#### Message Types

![[Pasted image 20230514140521.png]]


![[Pasted image 20230514140626.png]]



### Configuration

Configure with `# router ospf <process_id>`

A Router can run OSPF in multiple processes, but usually only singe process (usually 1) is used.

Process is unrelated to area

Area is set using `network <nw-addr> <wildcard-mask> area <area_number>`

network command tells the R which interfaces to activate OSPF on, it does not tell the R advertise these networks.

![[Pasted image 20230513222055.png]]

![[Pasted image 20230513222143.png]]


![[Pasted image 20230513222614.png]]

![[Pasted image 20230513222631.png]]

![[Pasted image 20230513222657.png]]

![[Pasted image 20230513222724.png]]

Doing `clear ip ospf process` is a bad idea in real networks

![[Pasted image 20230513222851.png]]


Configure OSPF and passive interfaces directly

![[Pasted image 20230514140739.png]]




# Quiz

![[Pasted image 20230513223330.png]]
(b & f)


![[Pasted image 20230513223415.png]]
(c)


![[Pasted image 20230513223539.png]]
1. 4
2. 3
3. 1


![[Pasted image 20230513223620.png]]
(b)


![[Pasted image 20230513223718.png]]
(a)


![[Pasted image 20230513223748.png]]
(a, d)



---

![[Pasted image 20230514140848.png]]
Down Init 2-way Exstart Exchange Loading Full
DoITwEELFully



![[Pasted image 20230514141113.png]]
(c)


![[Pasted image 20230514141209.png]]
(a)



![[Pasted image 20230514141243.png]]
(c)



![[Pasted image 20230514141324.png]]
(b)


![[Pasted image 20230514141409.png]]
(b)