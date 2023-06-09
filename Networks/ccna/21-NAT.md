
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


---

# Quiz

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
