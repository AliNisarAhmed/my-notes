# Ethernet Frame

## Ethernet Header

![[Pasted image 20230518201012.png]]


### Preamble & SFD

![[Pasted image 20230518201130.png]]


### Destination & Source

![[Pasted image 20230518201217.png]]


### Type or Length

![[Pasted image 20230518201340.png]]


### Length of the Header

![[Pasted image 20230518201401.png]]


## Ethernet Trailer

![[Pasted image 20230518201447.png]]


![[Pasted image 20230518201522.png]]


Ethernet Header length = 7 + 1 + 6 + 6 + 2 = 22 Bytes
Ethernet Trailer Length =                                   4 Bytes
Ethernet Frame Length =                                   26 Bytes

But usually, Header is counted without Preamble and SFD

Header length w/o Preamble and SFD            = 14 Bytes
Header + Trailer (w/o Preamble & SFD).         = 18 Bytes


## Ethernet Frame Size


![[Pasted image 20230518203732.png]]


---


# IPv4 Header


![[Pasted image 20230518203927.png]]


### Version

![[Pasted image 20230518204228.png]]



### IHL

![[Pasted image 20230518204629.png]]

![[Pasted image 20230518204705.png]]



### DSCP

![[Pasted image 20230518204747.png]]


### ECN

![[Pasted image 20230518204807.png]]



### Total Length


![[Pasted image 20230518204845.png]]


### Identification

![[Pasted image 20230518204939.png]]


### Flags

![[Pasted image 20230518205250.png]]


### Fragment Offset

![[Pasted image 20230518205358.png]]


### TTL

![[Pasted image 20230518205418.png]]


### Protocol

![[Pasted image 20230518205450.png]]



### Header Checksum


![[Pasted image 20230518205644.png]]

![[Pasted image 20230518205700.png]]


### Source & Destination IP Addresses

![[Pasted image 20230518205754.png]]


### Options

![[Pasted image 20230518205814.png]]

0 - 40 Bytes



