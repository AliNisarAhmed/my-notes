
# RIP

![[Pasted image 20230511211634.png]]

last point above^ is a problem with a lot of Rs because these regular updates can clog up the nw


### Versions

![[Pasted image 20230511211906.png]]

### network command

![[Pasted image 20230511212823.png]]

![[Pasted image 20230511212958.png]]

**NOTE**: the `network` command does not tell the R which nw to advertise.  It tells the R which interfaces to activate RIP on, and then the R will advert the nw prefix of those interfaces

Another example below:

![[Pasted image 20230511213123.png]]


### Passive Interfaces

Even when no RIP neighbors are connected (ex. R1 G2/0 above), R1 will continuously send RIP adverts out of that interface. 

This is unnecessary traffic, so any unused non-RIP interface should be configured as a **passive interface**, using the following command

`R1(config-router)# passive-interface <interface_id>`

![[Pasted image 20230511213422.png]]


### Advertise Default Routes

![[Pasted image 20230511213447.png]]

![[Pasted image 20230511213643.png]]

Note above: how "Gateway of last resort" is a single addr, while below it 2 addresses are mentioned as default route (asterisk).

RIP will actually load balance bw the 2 since they both have the same hop count, which is all that matters to RIP, even though one of them is a slower connection.

### RIP details in CLI

use `show ip route` to display the following info:

![[Pasted image 20230511213911.png]]

Notice:
- protocol is RIP
- RIP version 2
- automatic nw summarization is off
- maximum paths = 4
	- can be changed by `maximum-paths <1-32>`
	- specify how many paths for the same route can be entered in the R table
- nw Routed to
- passive interfaces
- RIP neighbors (10.0.12.2 and 10.0.13.2)
- AD (default 120)
	- can be changed by `distance <1-255>`



---


# EIGRP


![[Pasted image 20230511214311.png]]

### Configuration

![[Pasted image 20230511214522.png]]


### Wildcard Masks

Invert of Subnet mask

e.g `11111111.11111111.11111111.00000000` = `255.255.255.0` = `0.0.0.255` in wildcard mask

Just subtract `255` from each octet to get wildcard value

![[Pasted image 20230511214801.png]]

![[Pasted image 20230511214819.png]]

![[Pasted image 20230511214829.png]]

![[Pasted image 20230511214843.png]]


#### Wildcard mask matching

![[Pasted image 20230511215311.png]]
EIGRP activated above


![[Pasted image 20230511215328.png]]
EIGRP not activated above

![[Pasted image 20230511215233.png]]
EIGRP activated


![[Pasted image 20230511215428.png]]
EIGRP will be activated


### EIGRP Details in CLI


![[Pasted image 20230511215554.png]]

Notice above:
- proto is eigrp 1
- EIGRP uses interface b/w and delay as metric (K1, K3) (K2, K4, K5 are not used)
- Router ID
- Distance = 90 (170 external)

![[Pasted image 20230511215945.png]]



# Quiz

![[Pasted image 20230511220330.png]]

Answer: a

Explanation:

![[Pasted image 20230511220401.png]]



