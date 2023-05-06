#### Step 1: Selecting Root Bridge

- Switches use one field in the STP BPDU, the **Bridge ID** field, to elect a root bridge for the n/w
- The sw with the lowest Bridge ID becomes the root bridge
	- compare Bridge priority first
	- if equal, lowest MAC wins
- ALL ports on the Root bridge are marked as **Designated Ports** and put in a forwarding state, and other switches in the topology must have a path to reach the root bridge

###### Criteria for selection
- Root Bridge is the one with the lowest **Bridge ID**

**Bridge ID**
- Traditional
	- Bridge Priority (16 bits) + MAC Addr (48 bits)
	- Default Bridge Priority is 32768
- PVST (Per-Vlan ST)
	- Bridge Priority (16 bits) = Bridge Priority (4 bits) + Extended System Id aka VLAN ID (12 bits)
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
- Interfaces connected to Root Bridge - mark them as Non-Designated straight away

Therefore
- Each remaining collision domain will select ONE interface to be a designated port (forwarding state)
- The sw with the lowest root cost will make its port designated
- if the root cost is the same, the sw with the lowest **Bridge ID** will make its port designated
- The other sw will make its port non-designated (Blocking state)

###### Criteria for selection
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

# Router States

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

# SPT Timers

Once the network has converged, only the Root bridge sends out BPDUs

Routers only forward BPDUs to (out of) their Deisgnated Ports

1. Hello Timer
	- 2 sec
2. Forward Delay
	- 15 seco
3. Max Age
	- 20 sec (*10 sec)