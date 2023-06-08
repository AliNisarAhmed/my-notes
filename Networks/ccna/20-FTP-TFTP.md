
![[Pasted image 20230607211531.png]]


# TFTP

![[Pasted image 20230607211650.png]]


### TFTP Reliability

![[Pasted image 20230607211810.png]]
^ This method of reliability (the lock-step comms) is not as reliable as TCP's forward acknowledgement and sliding window, but gets the job done


### TFTP Connections

![[Pasted image 20230607211926.png]]


### TFTP ID

![[Pasted image 20230607212056.png]]
^ TFTP UDP port 69 is only used in the first message, thereafter the client and the server both start using each other's ephemeral ports



# FTP

![[Pasted image 20230607212646.png]]


### FTP Control Connections

![[Pasted image 20230607212948.png]]


Below: **NOTE**: Active/Passive mode only applies to Data connections because the client always initiates the control connection

#### Active Mode - FTP Data Connections

![[Pasted image 20230607213027.png]]
^ **NOTE**, the first arrow in TCP connection is going from server to the client
^ Also note that the FTP Control connection is maintained throughout this whole process
	- it is not terminated, so there are two connections at once


#### Passive Mode - FTP Data Connections

![[Pasted image 20230607213233.png]]
^ **NOTE** the client is behind a firewall, that is why the client has to initiate the connection



# Comparison

![[Pasted image 20230607213349.png]]



# IOS File System

![[Pasted image 20230607214353.png]]
^ `disk` is where usually the cisco IOS file itself is stored



# Upgrading Cisco IOS

![[Pasted image 20230607215507.png]]

![[Pasted image 20230607215521.png]]

![[Pasted image 20230607215540.png]]

![[Pasted image 20230607215556.png]]


![[Pasted image 20230607215629.png]]


## Command Review

![[Pasted image 20230607215654.png]]



---


# Quiz

![[Pasted image 20230607215936.png]]
(b, d)


![[Pasted image 20230607220000.png]]
(a)


![[Pasted image 20230607220023.png]]
(c)


![[Pasted image 20230607220240.png]]
(d)

![[Pasted image 20230607220257.png]]
(b, c)

![[Pasted image 20230607220353.png]]
(c, d)