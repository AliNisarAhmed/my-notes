
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

