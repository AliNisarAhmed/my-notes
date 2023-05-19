# STP

> Spanning Tree is a networking protocol that builds a loop-free logical topology for Ethernet networks
>
> Spanning tree also allows a network design to include backup links providing fault tolerance if an active link fails


#### Step 1: Selecting Root Bridge

- Switches use one field in the STP BPDU, the **Bridge ID** field, to elect a root bridge for the n/w
- The sw with the lowest Bridge ID becomes the root bridge
	- compare Bridge priority first
	- if equal, lowest MAC wins
- ALL ports on the Root bridge are marked as **Designated Ports** and put in a forwarding state, and other switches in the topology must have a path to reach the root bridge
	- (to be exact, Root Bridge has a Deisgnated port in *each collision domain* it is connected to, so if two interfaces are connced to a hub, it is possible for 1 of them to be Blocking/Alternate)

###### Criteria for selection
- Root Bridge is the one with the lowest **Bridge ID**

**Bridge ID**
- Traditional
	- Bridge Priority (16 bits) + MAC Addr (48 bits)
	- Default Bridge Priority is 32768
- PVST (Per-Vlan ST)
	- Bridge Priority (16 bits) = Bridge Priority (4 bits) + Extended System Id aka VLAN ID (12 bits) = Total = **64 bits**
	- MAC addr
	- With the default VLAN of 1, the default Bridge Priority is 32769 (32768 + 1)
	- Minimum unit of incr/decr of Bridge Priority = 4096

#### Step 2: Selecting Root Port

- Each remaining sw will select ONE of its interfaces to be root port
- Root ports are in a forwarding state
- The ports on the other interface connected to/opposite to the root ports are marked as Designated Ports

###### Criteria for selection
1. lowest **Root cost**
2. lowest neighbor **Bridge ID** (i.e: step 1 above)
3. Lowest neighbor **Port ID** 

**Root Cost**
- 10 Mbps = 100
- 100 Mbps = 19 (Fast Ethernet)
- 1 Gbps = 4
- 10 Gbps = 2

**Port ID**
- shown by the command `show spanning-tree`
- composed of `Priority` + `Port Number`
	- Priority defaults to 128
	- each interface gets a unique port number


#### Step 3: Selecting Designated ports in remaining collision domains

- Every collision domain must have a single STP Designated port

Therefore
- Each remaining collision domain will select ONE interface to be a designated port (forwarding state)
- The sw with the lowest root cost will make its port designated
- if the root cost is the same, the sw with the lowest **Bridge ID** will make its port designated
- The other sw will make its port non-designated (Blocking state)

###### Criteria for selection
- *Interfaces connected to Root Bridge - mark them as Non-Designated straight away*
- *Interfaces opposite root ports become deisgnated ports right away*

1. Interface on sw with the lowest **Root cost**
2. Interface on sw with the lowest **Bridge ID**




### Examples

---

![[Pasted image 20230504231935.png]]

Solution:

![[Pasted image 20230504232309.png]]


---

![[Pasted image 20230504232619.png]]


![[Pasted image 20230504232744.png]]


---

![[Pasted image 20230505165031.png]]

![[Pasted image 20230505165522.png]]


---
---
---

## Port States

Stable = { Forwarding, Blocking }
Transition = { Listening, Learning }

#### 1. Blocking
- Non-designated ports are in Blocking states
- DO NOT send/receive regular traffic
- DO receive STP BPDUs
- DO NOT forward STP BPDUs
- DO NO learn MAC address from regular traffic

#### 2. Listening
- Only Designated or Root ports enter Listening states (Non-Designated ports are always blocking)
- Duration = 15 seconds = **Forward Delay Timer**
- DO NOT send/receive regular traffic
- DO receive STP BPDUs
- DO forward STP BPDUs
- DO NOT learn MAC address from regular traffic

#### 3. Learning
- After Listening, Designated or Root ports will enter Learning state
- Duration = 15 seconds = **Forward Delay Timer**
- DO NOT send/receive regular traffic
- DO receive STP BPDUs
- DO forward STP BPDUs
- DO learn MAC addresses from regular traffic

#### 4. Forwarding
- Root and Designated ports are in a Forwarding state
- DO send/receive regualr traffic
- DO receive STP BPDUs
- DO forward STP BPDUs
- DO learn MAC addresses from regular traffic

#### 5. Disabled
- State of a shutdown (administratively disabled) interface

![[Pasted image 20230505194855.png]]


---

## STP Timers

Once the network has converged, only the Root bridge sends out BPDUs

Routers only forward BPDUs to (out of) their Deisgnated Ports

1. Hello Timer
	- 2 sec
2. Forward Delay
	- 15 seco
3. Max Age
	- 20 sec (10 x 2 sec)
	- indicates how long an interface will wait to change the STP topology (reevaluate STP choices, including root bridge, local root, designated and noin-desig ports) after ceasing to receive Hello BPDUs.
	- The timer is reset every time a BPDU is received
	- Once the timer expires, if a non-desig port is chosen to become a desig or root port, it will transition from Blocking to Listening state (15 sec), learning state (15 sec) and then finally forwarding state.
		- So it can take a total of **50 seconds** for a blocking interface to transition to forwarding state


---
---
---


# Rapid STP

An evolution of STP. In Cisco's own words

> RSTP is not a timer-based STP like 802.1D. Therefore, RSTP offers an improvement over the 30 seconds or more that 802.1D takes to move a link to forwarding state. The heart of the protocol is a new bridge-bridge handshake mechanism, which allows ports to move directly to forwarding state


## Similarities with STP

- RSTP serves the same purpose as STP
	- blocking specific ports to prevent Layer 2 loops
- RSTP elects a Root Bridge with the same rules as STP
	- i.e. lowest Bridge ID
- RSTP elects root ports with the same rules as STP
- RSTP elects the Designated ports with the same rules as STP

## Costs

![[Pasted image 20230506204350.png]]

10 Tbps = 2 (missing from above)

## Port States

In RSTP, Blocking & Disables states are combined into one, and Listening state is not used.

![[Pasted image 20230506204623.png]]


## Port Roles

- **Root port role** remains the same
	- The port that is closes to the root bridge becomes the root port for the sw
	- The root bridge is the only sw that does not have a root port
- The **Designated port** role remains unchanges in RSTP
	- The port on a segment (another name for collision domain) that sends the best BPDU is that segment's Designated port (only 1 per segment)
- The **Non-designated** (discarding/blocking) port is split into 2 separate roles in RSTP
	1. **Alternate port**
		- is a discarding port that receives a superior BPDU from another sw
		- same as blocking ports in STP
		- functions as *backup to the root port*
		- if the root port fails, the sw can immediately move its best alternate port to forwarding
	2. **Backup port**
		- a discarding port that receives a superior BPDUE from *another* *interface on the same sw*
		- This only happens when 2 interfaces are connected to the same collision domain (via a hub)
		- since hubs are no longer used, unlikely to be encountered in real life
		- Function as *backup for Deisgnated ports*
		- The interface with the lowest port ID will be selected as the Designated port, and the other will be the backup port


## Optional STP features built into RSTP

1. Uplink Fast
	- This immediate move to Forwarding state of an alternate port in case of root port failure functions like STP optional feature called *UplinkFast* (which is now built into RSTP with alternate ports)
2. BackboneFast
	- Backbone Fast is used to recover from an indirect link failure.
	- can minimize/nullify the max-age timer
3. PortFast
	- enabled via Edge Link Types (see Link Types below)


## Differences with STP

- RSTP is compatible with STP
	- the interface on RSTP sw connected to STP sw will operate in STP mode (i.e with timers, blocking -> listening -> forwarding process etc)
- Protocol version = 2 vs 0 for STP
- BPDU Type = 0x02 vs 0x00 for STP
- Uses all 8 bits of BPDU flags vs 2 bits for STP
- **Important**
	- In STP, only the root bridge originated BPDUs, and other sw just forwarded the BPDUs they recieved
	- In RSTP, ALL switches originate and send their own BPDUs from their Deisgnated ports
- Switches also "age" the BPDU info much quickly
	- in STP, a sw waits 10 hello intervals (i.e 20 seconds)
	- in RSTP, a sw considers a neighbor lost if it misses 3 BPDUs (6 seconds).
		- it will then flush all MAC addresses learne on that interface


## RSTP Link Types

RSTP distinguishes bw 3 different link types

1. **Edge**
	- a port that is connected to an end host. Moves directly to forwarding, w/o negotiation
	- this is **PortFast** from STP
	- the command to enable Edge link type manually is `spanning-tree portfast`
2. **Point-to-Point**
	- a direct connection b/w 2 sw
	- function in full-duplex mode
	- sw detects that is connected to another sw and enables point-to-point
	- however, the command to do manually is `spanning-tree link-type point-to-point`
3. **Shared**
	- a connection to a hub. 
	- Must operate in half-duplex mode
	- auto detected, but can be manually configured using `spanning-tree link-type shared`


## Example

![[Pasted image 20230506215218.png]]

Solution: 

1. Identify Root Bridge & Root ports
	- notice SW4's G0/0 becomes Root Port as SW3's cost is lower (8193 vs 32769)
![[Pasted image 20230506215804.png]]

2. Mark Designated Ports
	- notices SW2's G0/1 becomes Designated as SW2 has lower cost vs SW4
![[Pasted image 20230506215813.png]]

3. Identify Alternate/Backup ports
![[Pasted image 20230506215822.png]]

4. Identify Link-types
![[Pasted image 20230506215705.png]]

