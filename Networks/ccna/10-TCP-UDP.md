
# TCP

![[Pasted image 20230518222059.png]]


### Functions 


![[Pasted image 20230518222211.png]]


![[Pasted image 20230518222745.png]]


#### Session Multiplexing

A Session is an exchange of data b/w two or more communicating devices

*Session multiplexing* or *Connection Multiplexing* allows multiple applications to send and receive data simultaneously.

It assigns a unique number to each individual session or connection to keep it separate from others. 
- this unique number is known as port number

In each session or connection, two port numbers are used; source port and destination port. **Source port number** is used to identify the session or connection while **destination port number** is used to identify the application that processes the data at destination host.

The port number field is 16 bits in length that allows a total of 65536 (from 0 to 65535) port numbers. Port numbers are divided in three categories; 
1. **well-known**
	- 0 - 1023
2. **registered**
	- 1024 - 49151
3. **dynamically assigned** or **ephemeral**
	- 49152 - 65535


![[Pasted image 20230518222622.png]]


### Header


#### Source Port & Destination Port (16 bits each)

![[Pasted image 20230518222806.png]]


#### Sequencing number & Ack number (32 bits each)

![[Pasted image 20230518222922.png]]


#### Flags: ACK, SYN, FIN (1 bit each)

![[Pasted image 20230518222954.png]]


#### Window Size (16 bits)

![[Pasted image 20230518223006.png]]




## Three-way & Four-way Handshakes

![[Pasted image 20230518223137.png]]

![[Pasted image 20230518223208.png]]


## Sequencing / Acknowledgement

TCP uses *Forward Acknowledgement*

![[Pasted image 20230518223319.png]]


![[Pasted image 20230518223354.png]]


## Flow Control

![[Pasted image 20230518223426.png]]



---


# UDP

![[Pasted image 20230518223511.png]]



---


# Comparison

![[Pasted image 20230518223523.png]]


![[Pasted image 20230518223544.png]]

![[Pasted image 20230518223551.png]]


---


# Common Port Numbers

![[Pasted image 20230518223633.png]]


---


# Quiz

![[Pasted image 20230518223716.png]]
(a)


![[Pasted image 20230518223733.png]]
(c)

![[Pasted image 20230518223803.png]]
(b, d, e)


![[Pasted image 20230518223840.png]]
(a, c, f)


![[Pasted image 20230518223922.png]]
(c)


![[Pasted image 20230518224000.png]]


![[Pasted image 20230518224031.png]]


