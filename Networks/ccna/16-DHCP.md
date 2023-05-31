
# DHCP

![[Pasted image 20230530211328.png]]


## Basic Functions

![[Pasted image 20230530211359.png]]
^**NOTE**: (Preferred) after IP address
- This means this PC was previously assigned this IP address by the DHCP server, so it asked to receive the same address again (provided the address is still available)
^**NOTE**: also notice the Lease Obtained and Lease Expires date
- DHCP servers "lease" IP address to clients
- These leases are usually not permanent, and the client must give up the address at the end of the lease
^**NOTE**: The default gateway, DHCP Server and DNS Server values were all set by the DHCP server

![[Pasted image 20230530212549.png]]

![[Pasted image 20230530212720.png]]

## Ports

DHCP Servers use **Port 67**
DHCP Clients use **Port 68**



## DHCP Messages

![[Pasted image 20230530212840.png]]

![[Pasted image 20230530215658.png]]
**NOTE**: Use DORA to remember the messages

### 1. DHCP Discover

Broadcast message from the client, asking if there are any DHCP servers in the network

![[Pasted image 20230530212926.png]]
^**NOTE**: the source IP address of 0.0.0.0, the IP used when the IP is not yet assigned to the client
^**NOTE**: the Destination IP of 255.255.255.255, the broadcast address
^**NOTE**: DHCP Discover message field:
- BootP Flags
	- BootP is the predecessor of DHCP
	- Notice Unicast: The client wants the offer message to be unicast
- Client IP = 0.0.0.0
- Client MAC is displayed
- Options
	- Requested IP Address (192.168.0.167) - the IP this PC had before and wants to receive the same IP again


### 2. DHCP Offer

Message from DHCP Server to the Client

DHCP offer  message can be *broadcast* or *unicast*
- Depends on how the DHCP clients wants the offer message to be returned
- Some clients will not accept a Unicast message until their IP address is configured


![[Pasted image 20230530214629.png]]
^**NOTE**: Unicast Frame to Client's MAC address
^**NOTE**: Unicast at  IP layer as well, the destination is the IP address being offered to the client
**NOTE**: Server and Client  ports
**NOTE**: BootP flags - note the unicast, since the client requested unicast in DHCP Discover message


### 3. DHCP Request

A message from the Client saying I want to use the IP address you offered to me

There may be multiple DHCP servers each of which may have replied to Client's Discover message, so the Client needs to specify from which server it is accepting the Offer from.

![[Pasted image 20230530215228.png]]
**NOTE**: Destination MAC = Broadcast - to cater to possible multiple DHCP Servers
**NOTE**: IP is also broadcast
**NOTE**: Option 54, the server IP address to identify the Offerer


### 4. DHCP Ack

Server to client confirming the use of the requested IP address.

DHCP Ack can be *broadcast* or *unicast* depending on what the client requested


![[Pasted image 20230530215402.png]]
**NOTE**: Unicast addresses (MAC and IP)


## DHCP Relay

![[Pasted image 20230530215746.png]]

![[Pasted image 20230530215837.png]]


## Configuration

![[Pasted image 20230530220328.png]]

![[Pasted image 20230530220446.png]]

![[Pasted image 20230530220550.png]]

![[Pasted image 20230530221007.png]]

### Summary

![[Pasted image 20230530221021.png]]


---

# Quiz


![[Pasted image 20230530221909.png]]
(b) - remember by DORA


![[Pasted image 20230530221940.png]]
(d)


![[Pasted image 20230530222350.png]]
(d) - notice the BootP Broadcast flag - indicating that this message was broadcast


![[Pasted image 20230530222540.png]]
(a, c, e) - (b, d) are always broadcast


![[Pasted image 20230530222637.png]]
(a)


![[Pasted image 20230530222934.png]]
(f)