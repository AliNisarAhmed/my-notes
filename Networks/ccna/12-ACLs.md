
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


## Global Config mode vs Name Mode

ACLs can be added in two different modes
1. From Global Config mode
	- `R1(config)# access-list 1 deny 192.168.1.1`
2. From ACL Config mode by using the "named" config mode and entering subcommands
	- `R1(config)# ip access-list standard 1`
	- `R1(config-std-nacl)# deny 192.168.1.1`
	- `R1(config-std-nacl)# permit any`

**NOTE**: Numbered ACLs, even when configured with "named" mode, will appear in running-config as if configured in the standard mode for them (Global Config method)


### Advantages of Named Mode

1. Can delete individual entries
![[Pasted image 20230525214543.png]]
vs
![[Pasted image 20230525214618.png]]


2. can insert new entries in between other entries by specifying the sequence number

![[Pasted image 20230525214749.png]]


##  Resequencing ACLs

![[Pasted image 20230525215057.png]]




# Types of ACLs


## Standard ACLs

![[Pasted image 20230524213038.png]]



### 1. Standard Numbered ACLs



![[Pasted image 20230524214440.png]]
^ the first 3 are different ways for /32
^ the next 2 are different ways to allow all traffic


#### Example

![[Pasted image 20230524215909.png]]

![[Pasted image 20230524220409.png]]
^ PC1 tries to ping PC3 - permitted

![[Pasted image 20230524220438.png]]
^ PC2 tries to ping PC3 - denied 



### 2. Standard Named ACLs



![[Pasted image 20230524223708.png]]

![[Pasted image 20230524224008.png]]


#### Example

![[Pasted image 20230524224134.png]]

First ACL
![[Pasted image 20230524224154.png]]

Second ACL
![[Pasted image 20230524224207.png]]


**NOTE** The order of the ACEs differs from the sequence number, Reason below:
![[Pasted image 20230524224407.png]]



## Extended ACLs

![[Pasted image 20230525215317.png]]

**Rule of Thumb**: Extended ACLs should be applied as close to the source as possible
- this is because compared to Standard ACLs, Extended ACLs are very specific
	- Rs need more resources to apply these ACLs
	- plus, there is a bug chance to block large traffic if something goes wrong unintentionally while configuring the Extended NACL


### Matching the protocol

![[Pasted image 20230525215747.png]]
^ can use protocol IP number or its name
**NOTE**: the `ip` option matches all IP packets
- we use this when we do not care about protocol and just want to permit or deny all packets regardless of protocol


### Matching the source/dest IP addr

![[Pasted image 20230525220014.png]]


#### Practice

![[Pasted image 20230525220246.png]]
1. `permit ip any any`
2. `deny udp 10.0.0.0 0.0.255.255 host 192.168.1.1`
3. `deny icmp 172.16.1.1 0.0.0.0 192.168.0.0 0.0.0.255`
	- or `deny icmp host 172.16.1.1 192.168.0.0 0.0.0.255`


### Matching the TCP/UDP Port numbers

![[Pasted image 20230525221534.png]]

![[Pasted image 20230525221743.png]]

#### Practice

![[Pasted image 20230525221819.png]]
1. `permit tcp 10.0.0.0 0.0.255.255 host 2.2.2.2 eq 443`
2. `deny udp any range 20000 30000 host 3.3.3.3`
3. `permit tcp 172.16.1.0 0.0.0.255 gt 9999 4.4.4.4 0.0.0.0 neq 23`


### Example

![[Pasted image 20230525222520.png]]


![[Pasted image 20230525223255.png]]

![[Pasted image 20230525223337.png]]


![[Pasted image 20230525223439.png]]
^ applied to R1

Q: Find a more efficient solution to above





---

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

---


![[Pasted image 20230525223633.png]]
(4 = ACL# 103)



![[Pasted image 20230525223838.png]]
(b)


![[Pasted image 20230525223946.png]]
(c)


![[Pasted image 20230525224129.png]]
(3, ACL 112)


![[Pasted image 20230525224329.png]]
(c, e)


![[Pasted image 20230525224644.png]]
^note: cloud is public internet
(a)