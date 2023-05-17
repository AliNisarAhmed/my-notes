A loopback interface is a virtual interface in the router (ie it actually does not exist physically)

It is always up/up (unless manuall shutdown)

It is not dependent of a physical interface

It is used as an "address" for a R even when one or more of its physical interfaces fail
- Thus it provides a consistent IP address that can be used to reach the R
	- as long as at least 1 physical interface is working


![[Pasted image 20230516190713.png]]

