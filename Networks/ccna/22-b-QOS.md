
# QOS

## Introduction

![[Pasted image 20230610214717.png]]

![[Pasted image 20230610214754.png]]


## Characteristics managed by QOS

![[Pasted image 20230610215342.png]]


## QOS Standards

![[Pasted image 20230610215422.png]]


## Queuing

![[Pasted image 20230610215513.png]]


#### Problem of Tail Drop and TCP Global Synchronization

![[Pasted image 20230610215649.png]]


#### Solution: RED & WRED

![[Pasted image 20230610215813.png]]



## Classification


![[Pasted image 20230613194900.png]]


### PCP & CoS

![[Pasted image 20230613195042.png]]
^ To **mark** traffic means to set the value in the PCP or DSCP fields.
- then the network devices look at those markings and use them to classify the traffic as high-priority, low-priority etc
- So when an IP phone marks its traffic as PCP5, it's coz it wants the routers and switches to classify those packets as high-priority

![[Pasted image 20230613195424.png]]
^ Only the traffic originating from the phones will be marked with PCP, since it is the only traffic in the network above that is tagged with a VLAN


### DSCP & ToS

ToS = Type of Service

![[Pasted image 20230613204510.png]]

![[Pasted image 20230613204610.png]]

![[Pasted image 20230613204809.png]]

![[Pasted image 20230613205747.png]]


![[Pasted image 20230613205833.png]]

![[Pasted image 20230613210942.png]]

![[Pasted image 20230613211022.png]]

![[Pasted image 20230613211045.png]]

![[Pasted image 20230613211134.png]]

![[Pasted image 20230613211151.png]]

![[Pasted image 20230613211207.png]]

Formula for above conversions: 

> DSCP = 8X + 2Y where AFXY is the AF value in decimal


![[Pasted image 20230613211325.png]]




---


# Quiz

![[Pasted image 20230610215833.png]]
(a, d)


![[Pasted image 20230610215931.png]]
(c)

![[Pasted image 20230610215957.png]]
(b, c, e)


![[Pasted image 20230610220034.png]]
(d)


![[Pasted image 20230610220050.png]]
(a)

![[Pasted image 20230610220124.png]]
(d) - RED is random, while WRED drops based on traffic class
