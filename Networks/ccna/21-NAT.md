
# Private IPv4 Addresses


![[Pasted image 20230608210551.png]]

![[Pasted image 20230608210736.png]]



# NAT

![[Pasted image 20230608211204.png]]


### Static NAT

![[Pasted image 20230608211539.png]]


![[Pasted image 20230608211732.png]]
^**Note**: Using static NAT, PC2 is not allowed to be translated the IP address used by PC1
- thus, each device needs its own public IP address, defeating the whole purpose of private IP addresses
- That is why static NAT is not often used
- If another IP is attempted to be translated, that command will be rejected (see quiz q#2)


**NOTE**: The one-to-one mapping of static NAT allows external hosts to access the internal host via the inside global address


#### Configuration


![[Pasted image 20230608212123.png]]

![[Pasted image 20230608212220.png]]
^**Note**
**Inside/Outside** = Location of the host
**Local/Global** = Perspective

![[Pasted image 20230608212446.png]]


![[Pasted image 20230608212554.png]]


#### Command Summary

![[Pasted image 20230608212542.png]]


### Dynamic NAT

![[Pasted image 20230609205448.png]]
^ Although mappings are dynamically assigned, the mappings are still one-to-one (*one inside local* IP address per *one inside global* IP address)


#### NAT Pool Exhaustion

If there are not enough *inside global* IP addresses available (i.e. all are being used), it is called *NAT pool exhaustion*

**NOTE**: if a packet from another inside host arrives and needs NAT but there are no available addresses (i.e NAT pool exhausted), **the packet will be dropped**
- the host will be unable to access outside networks *until* one of the inside global IP addresses becomes available
- Dynamic NAT entries will time out automatically if not used
	- or they can be cleared manually


#### Configuration

![[Pasted image 20230609210341.png]]

![[Pasted image 20230609210426.png]]

![[Pasted image 20230609210438.png]]




### PAT (NAT Overload)

![[Pasted image 20230609210808.png]]
^**NOTE**: in the above example, PC1 and PC2 happen to choose the same port, that is why PAT was needed, had they chosen different ports PAT would not be needed, as the IP/Port combo needs to be unique


**NOTE**: Because many inside hosts can share a single public IP, PAT is very useful for preserving public IP addresses, and it is used in networks all over the world


#### Configuration

![[Pasted image 20230609211123.png]]

![[Pasted image 20230609211205.png]]


#### PAT Configuration (interface method)

In this method, we configure the Router to use its own public IP address as the global inside address for NAT mappings

![[Pasted image 20230609211422.png]]

![[Pasted image 20230609211504.png]]

![[Pasted image 20230609211523.png]]


#### Command Summary

![[Pasted image 20230609211543.png]]



---

# Quiz 1

![[Pasted image 20230608212722.png]]
(d)


![[Pasted image 20230608212814.png]]
(b)


![[Pasted image 20230608212951.png]]
(b)


![[Pasted image 20230608213008.png]]
(a, e, f)


![[Pasted image 20230608213110.png]]
Outside Global = 8.8.8.8
Outside Local   = 8.8.8.8
Inside Local      = 172.20.0.101
Inside Global     = 200.0.0.1


![[Pasted image 20230608213324.png]]
(f)


---


# Quiz 2


![[Pasted image 20230609211620.png]]
(d)

![[Pasted image 20230609211636.png]]
(b)
- in (a) the last address 203.0.113.255 is outside of /25 subnet
- in (c), the ACL is specified with a subnet mask, whereas ACLs use wildcard mask


![[Pasted image 20230609212031.png]]
(b)


![[Pasted image 20230609212105.png]]
(a)


![[Pasted image 20230609212227.png]]
(c)


![[Pasted image 20230609212623.png]]
(c)
