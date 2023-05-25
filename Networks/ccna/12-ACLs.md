
# How ACLs work


![[Pasted image 20230524211330.png]]

![[Pasted image 20230524211408.png]]

![[Pasted image 20230524211453.png]]


## Inbound vs Outbound

![[Pasted image 20230524211954.png]]
^ Outbound on G0/2 does not meet the requirement
^**Note** that traffic entering from PC3 into G0/1 will not be checked at all since ACL is outbound

![[Pasted image 20230524212139.png]]
^ Inbound on G0/2 does not meet the requirement either
^  No traffic can go through R1 at all because all IN traffic is being dropped from both PC3 and PC4 (these PCs are effectively cut off from the nw)

![[Pasted image 20230524212330.png]]
^ Outbound on G0/1 of R2 meets the requirements

**Rule of thumb**: apply ACLs as close to the destination as possible

## ACE Order

ACE Order is important, because the Router stops at the first ACE that matches the condition, ignoring the rest of them.

![[Pasted image 20230524212546.png]]
^ Reverse the ACEs and the traffic will be blocked


## Maximum ACEs

A maximum of one ACL can be applied to a single interface *per direction*
**Inbound**: Maximum 1 ACL
**Outbound**: Maximum 1 ACL

If you apply a second ACL on top of another, the existing one will be dropped and the latest one will become in effect


## Implicit Deny

![[Pasted image 20230524212930.png]]


# Types of ACLs

![[Pasted image 20230524213038.png]]


## 1. Standard Numbered ACLs

![[Pasted image 20230524214440.png]]
^ the first 3 are different ways for /32
^ the next 2 are different ways to allow all traffic


### Example

![[Pasted image 20230524215909.png]]

![[Pasted image 20230524220409.png]]
^ PC1 tries to ping PC3 - permitted

![[Pasted image 20230524220438.png]]
^ PC2 tries to ping PC3 - denied 


## 2. Standard Named ACLS

![[Pasted image 20230524223708.png]]

![[Pasted image 20230524224008.png]]


### Example

![[Pasted image 20230524224134.png]]

First ACL
![[Pasted image 20230524224154.png]]

Second ACL
![[Pasted image 20230524224207.png]]


**NOTE** The order of the ACEs differs from the sequence number, Reason below:
![[Pasted image 20230524224407.png]]


# Quiz

![[Pasted image 20230524224742.png]]
(a: top left)

![[Pasted image 20230524224802.png]]
(g0/2 outbound)


![[Pasted image 20230524224907.png]]
(b) - because the last ACL applied was ACL#10 which overrides the rest


![[Pasted image 20230524225045.png]]
(d) (and NOT b), coz the ACL is inbound, all the rules will be checked when ICMP Response is coming back from Server, at which point `permit any` will be the only ACE that will take effect


![[Pasted image 20230524225328.png]]
(c)


![[Pasted image 20230524225428.png]]
(b)
