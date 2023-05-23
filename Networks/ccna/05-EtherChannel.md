
### ASW vs DSW

Access Swtiches -> To which End hosts connect to
Distrubution Layer Switches -> A SW that access layer switches connect to

# EtherChannel


EtherChannels solve the problem of network congestion by *oversubscription*.

EtherChannels also known as:
	- Port Channels
	- LAG (Link Aggregation Group)

**OverSubscription**:
> When the bandwidth of the interfaces connected to end hosts is > than the b/w of the connection to the distribution switches
> 
> Some oversubscription is acceptable, but too much causes congestion

If we connect two sw together with multiple links w/o setting up EtherChannel, all except one will be disabled by SPT

EtherChannel is represented by the circle in the diagram below

![[Pasted image 20230507112753.png]]


## Load Balancing

Traffic using EtherChannel will be **Load Balanced** among the physical interfaces in the group. 

An algo is used to determine which traffic will use the interface.

EtherChannel load balances based on **flows**

A **flow** is communication between two nodes in the same network

Frames in the same flow will be forwarded using the same physical interface.
- If frames in the same flow were forwarded using different physical interfaces, some frames may arrive out of order at the destination, which can cause problems

You can change the inputs used in the interface selection calculation

### Inputs
- Source MAC
- Destination MAC
- Source AND Destination MAC
- Source IP
- Desti. IP
- Source AND Destination IP


## EtherChannel Config

3 methods

1. PAgP (Port Aggregation Protocol)
	- Cisco proprietary
	- dynamically negotiates creation/maintenance of the EtherChannel (like DTP for trunks)
	- 8 interfaces per EtherChannel
	- will form EtherChannel with `desirable-desirable` or `desirable-auto`
	- Will not form EtherChannel with `auto-auto` 
1. LACP
	- Industy Standard (IEEE 802.3ad)
	- works same (dynamically) as PAgP
	- 8 interfaces per EtherChannel
		- allows 16 but only 8 will be active
		- others in standby mode
	- will form EtherChannel with `active-active`
	- `passive` does not form an EtherChannel
2. Static EtherChannel
	- no protocol is used to determine if EtherChannel should be formed
	- interfaces are statically configured to form an EtherChannel
	- usually not used
	- will form EtherChannel with `on-on`

Member interfaces must have matching configs:
- same duplex
- same speed
- same switchport mode (access/trunk)
- same allowed VLAN/native VLAN (for trunk interfaces)

If an individual interfaces config does not match, it wll be excluded from EtherChannel

**Note**: Once EtherChannel is formed, STP treats that etherChannel as a single logical interface

### Ether Port flags

D = down
U = up (good)
P = bundled in port-channel (good) vs I = Standalone (bad, not connected)
s = suspended
S = Layer 2
R = Layer 3


## Layer3 EtherChannels

STP can cause Broadcast Storms even using EtherChannels

That is why, if the connections between interfaces are made with layer 3 Routing ports, and not switchports, there is no need to use STP at all, thus loops and storms are avoided.

Routed ports do no forward Layer 2 broadcasts, so no Layer 2 loops can be formed.



## Commands

[[01-ios-commands#EtherChannel]]


# Quiz

![[Pasted image 20230522104609.png]]
(a, c, g)


![[Pasted image 20230522105155.png]]
(b)

![[Pasted image 20230522105219.png]]
(c, d)


![[Pasted image 20230522105301.png]]
(b) - coz `on` is used with `pagp`
