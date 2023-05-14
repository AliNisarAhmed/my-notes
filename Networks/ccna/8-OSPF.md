
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

