
## LAN

A single *broadcast domain,* including all devices in that broadcast domain

*Broadcast Domain*
>A broadcast domain is the group of devices which will receive a broadcast frame (destination MAC `FFFF.FFFF.FFFF`) sent by any one of the members


![[Pasted image 20230507224630.png]]

^ 4 LANs above


## VLAN

VLANs logically split a network by separating end hosts at Layer 2. 

VLANs are used to split broadcast domains

VLANs are configured on a per-interface basis

The SW does not perform inter-VLAN routing, it must send traffic through the Router (or a Layer 3 SW)

VLANs 1, 1002-1005 exist by default and **cannot be deleted*


## Access Ports vs Trunk Ports

> An access port is a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs

vs

> Switches which carry multiple VLANs are called trunk ports


## Quiz 1

1. How many broadcast domains

![[Pasted image 20230507225348.png]]

![[Pasted image 20230507225401.png]]


2. How many broadcast domains

![[Pasted image 20230507225427.png]]


![[Pasted image 20230507225448.png]]

3. What happens if you try to assign a SW interface to a VLAN that does not exist?
- Answer - The switch creates the VLAN

4. PC3 sends broadcast msg, how many devices receive it?

![[Pasted image 20230507225727.png]]


![[Pasted image 20230507225741.png]]



---
---
---

## Trunk Ports

> Switchports which carry traffic from multiple VLANs are called Trunk ports

**Reason of use**: 
- In a small network with a few VLANs, it is possible to use a separate interface for each VLAN when connecting switches to switches, and switches to routers
- However, when the number of VLANs increases, this is not viable. 
	- It will result in wasted interfaces, and often routers do not have enough interfaces for each VLAN

A sw allows all VLANs by default, but can be configured to allow only certain VLANs

### Tagging

Switches tag all frames that they send over a trunk link.

This allows the receiving sw to know which VLAN the frame belongs to.

Which is why Trunk ports are also known as **tagged ports**.
- access ports are known as **untagged ports**


### Protocols

#### 1. **ISL**
- Inter-Switch Link
- Cisco proprietary
- not in use anymore


#### 2. **IEEE 802.1Q** 
- aka dot1q
- Industry standard
- dot1q tag comes Source MAC address and Type/Length in the Ethernet header
- Tag consists of two main fields
	- Tag Protocol Identifier (TPID)
	- Tag Control Information (TCI)

TOTAL = **32 bits**
![[Pasted image 20230508203352.png]]


##### TPID
- 16 bits (2 bytes)
- Always set to `0x8100`
	- indicating that the frame is 802.1Q tagged

##### PCP
- 3 bits
- Used for Class of Service (CoS)
	- which prioritizes important traffic in congested networks

##### DEI
- 1 bit
- used to indicate frames that can be dropped if the n/w is congested

##### VID
- 12 bits
- Identifies the VLAN the frame belongs to
- 12  bits = 4096 VLANs
- 0 and 4095 VLANs are reserved and cannot be used
- therefore, actual range = 1 - 4094

![[Pasted image 20230508210021.png]]



##### VLAN Ranges

- Valid VLAN range (1-4094) is divided into two sections
	1. Normal VLANs (1-1005)
	2. Extended VLANs (1006-4094)

Some older devices cannot use extended VLANs, however, it is safe to assume that modern switches will support extended VLAN range


##### Native VLAN

Native VLAN is `1` bu default, but can be manually configured

The sw *DOES NOT* add a 802.1Q tag to frames in Native VLAN
- When a sw receives an untagged frame on a trunk port, it assumes that frame belongs to the Native VLAN

**Note**: For security purposes, it's best to change Native VLAN to an unused VLAN
- make sure the native VLAN matches on all sw


###  Router on a Stick (ROAS)

ROAS allows us to divide a Router's physical interface into multiple sub-interfaces which will allow us to perform inter-VLAN routing using only 1 physical interface

ROAS is used to route bw multiple VLANs using a single interface on the router and the sw
- the sw is configured as a regular trunk
- The router interface is configured using **sub-interfaces**
	- each with its own VLAN tag and ip address
- The router will behave as if the frames arriving with a certain VLAN tag have arrived on the sub-interface configured with that VLAN tag
- The router will tag frames sent out of each sub-interface with the VLAN tag configured on the sub-interface

Written with a dot notation e.g. `g0/0.10` where the last number usually matches the VLAN id the interface is for.

![[Pasted image 20230508205258.png]]


Two methods of configuring the native VLAN on a ROAS:
1. use command `encapsulation dot1q <vlan_id> native`
2. Configure the IP addr for the native VLAN on the router's physical interface



## Layer 3 SW for inter-VLAN routing

- A multi-layer sw is capable of both switching and routing
- You can assign IP addr to its interfaces, like a Router
- You can create Virtual Interfaces for each VLAN, and assign IP add to those fields
	- by first doing `no switchport`, and then assign IPs
- You can configure routes on it
- It can be used for inter-VLAN routing
	- enabled by `ip routing` command


### SVI

- Switch Virtual Interfaces SVIs are the VIFs you can assign IP addr to in a Layer 3 sw
- configure each PC to use the SVI (and NOT the Router as in ROAS) as their default gateway
- To send traffic to different VLANs, the PCs will send traffic to the sw, and sw will route the traffic
- A Router then is used as a default route for e.g. Internet access
- SVI are *shutdown* by default
	- use `no shutdown` to enable

Conditions for Routing
- The VLAN assigned to SVI must exist
- sw must have at least one access port in the VLAN in an  *up/up* state, AND/OR 1 trunk port that allows the VLAN that is in `up/up` state


## Quiz 2

1. configure SW1 to send VLAN10 frames untagged, command?
	- `switchport trunk native vlan 10`
2. return a trunk interface to its default state?
	- `switchport trunk allowed vlan all`
3. `switchport mode trunk` rejected, which command may fix this?
	- `switchport trunk encapsulation dot1q`
4. Configure native VLANs on a Router in ROAS?
	1. `R1(config-subif)# encapsulation dot1q 112 native` and then `ip addre xxxx`
	2. `R1(config-if)# ip address <ip>`