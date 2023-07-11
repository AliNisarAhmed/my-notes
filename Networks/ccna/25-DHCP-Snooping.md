
# DHCP Snooping

## How it works

![[Pasted image 20230703205215.png]]
^ i.e., a SW trusts a DHCP messages received from a Router, but may or may not coming from PCs

![[Pasted image 20230703205243.png]]

![[Pasted image 20230703205303.png]]


## vs DHCP based attacks

### DHCP Starvation

![[Pasted image 20230709152621.png]]


### DHCP Poisining

![[Pasted image 20230709152739.png]]


![[Pasted image 20230709152810.png]]

![[Pasted image 20230709152819.png]]

![[Pasted image 20230709152909.png]]

![[Pasted image 20230709152922.png]]

![[Pasted image 20230709152946.png]]


## DHCP Messages

![[Pasted image 20230709153051.png]]
^ DHCP Server messages received on an untrusted port will always be discarded with no further checks
^ DHCP client messages will be inspected, and then the switch will decide to forward or discard the frame


## DHCP Snooping Operations

![[Pasted image 20230709153407.png]]


## Configuration

![[Pasted image 20230709154032.png]]


## DHCP Snooping Rate-Limiting

![[Pasted image 20230709154145.png]]

![[Pasted image 20230709154414.png]]


## DHCP Option 82 (Information Option)

![[Pasted image 20230709202703.png]]

![[Pasted image 20230709202812.png]]

![[Pasted image 20230709202822.png]]


--- 


# Quiz

![[Pasted image 20230709203449.png]]
(c, d, g) i,e server msg types


![[Pasted image 20230709203534.png]]
(d)

![[Pasted image 20230709203544.png]]
(a, c)

![[Pasted image 20230709203626.png]]
(a, b)

![[Pasted image 20230709203644.png]]
(b)

![[Pasted image 20230709203836.png]]
(e)
