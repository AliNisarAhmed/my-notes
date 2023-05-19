
# First Hop Redundancy Protocols


> An FHRP is a networking protocol which is designed to protect the default gateway used on a subnet by allowing 2 or more Rs to provide backup for that addr; 
> 
> in the event of failure if an active R, the backup R will take over the addr, usually within a few seconds


## Summary

![[Pasted image 20230517205909.png]]


## How it works

![[Pasted image 20230517210039.png]]
^Scenario: R1 is configured as default gw but it has just gone down (maybe due to hw 
failure)

![[Pasted image 20230517210321.png]]
^FHRP setup: Rs use a Virtual IP of `.252`, with one R acting as `Active | Master` and the other one acting as `Standby | Backup`

![[Pasted image 20230517210426.png]]

![[Pasted image 20230517210447.png]]

![[Pasted image 20230517210517.png]]
^**Note**: ARP Reply uses a Virtual MAC Address

![[Pasted image 20230517210631.png]]


![[Pasted image 20230517210641.png]]
^ R1 has gone down, and R2 assumes the role of Active|Master Router


![[Pasted image 20230517210813.png]]
^ PC1's ARP table is correct (since R2 assumes both VIP and VMAC), however, the switches' ARP table needs to be updated by R2, that's where **Gratuitous ARP** comes in


### Gratuitous ARP

> ARP replies sent without being requested (ie no ARP request was received for it)
>
> The frames are broadcast to `FFFF.FFFF.FFFF` (normal ARP replies are unicast)


![[Pasted image 20230517211123.png]]
^ continued from last diagram

![[Pasted image 20230517211145.png]]

![[Pasted image 20230517211201.png]]
^ After the gratuitous ARP, the packet will be sent to the correct Active R

![[Pasted image 20230517211315.png]]
^ **NOTE**: if R1 comes back online, it will not automatically become Active R again



## 1. HSRP


![[Pasted image 20230517211543.png]]

![[Pasted image 20230517211607.png]]

**NOTE**: *CANNOT* load balance between Routers in the same VLAN 

## 2. VRRP

![[Pasted image 20230517211712.png]]

![[Pasted image 20230517211937.png]]

**NOTE**: *CANNOT* load balance between Routers in the same VLAN


## 3. GLBP

![[Pasted image 20230517212012.png]]


## Comparison

![[Pasted image 20230517212031.png]]



## HSRP Configuration

![[Pasted image 20230517213645.png]]

![[Pasted image 20230517213724.png]]

![[Pasted image 20230517213740.png]]

![[Pasted image 20230517213754.png]]


# Quiz

![[Pasted image 20230517214310.png]]
(d)

![[Pasted image 20230517215631.png]]
(a)

![[Pasted image 20230517215713.png]]
(b, d)

![[Pasted image 20230517215830.png]]
(b)

![[Pasted image 20230517215922.png]]
(c)


![[Pasted image 20230517215958.png]]
(c, d)