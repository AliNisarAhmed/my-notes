
# SNMP

Stands for *Simple Network Management Protocol*

![[Pasted image 20230531210745.png]]
^ SNMPv3 is not that simple


### TLDR

![[Pasted image 20230531213847.png]]


## Operations

### 1. Notify Events

![[Pasted image 20230531210856.png]]


### 2. Status queries

![[Pasted image 20230531210939.png]]


### 3. Change config

![[Pasted image 20230531211010.png]]


## Components

![[Pasted image 20230531211044.png]]


### 1. NMS

![[Pasted image 20230531211117.png]]
Many free and commercial SNMP applications available


### 2. Managed Devices

Switches and Routers

![[Pasted image 20230531211219.png]]


## SNMP OIDs

![[Pasted image 20230531211310.png]]
OIDs identify "configuration properties" on the SNMP client, which the NMS can query and ask to change. 

Each OID is identified by this OID format as shown above

There are many OIDs, visit the page above to see them


## SNMP Versions

![[Pasted image 20230531211447.png]]

## SNMP Messages

![[Pasted image 20230531211456.png]]


### 1. Read

![[Pasted image 20230531211557.png]]


### 2. Write

![[Pasted image 20230531211633.png]]


### 3. Notification

![[Pasted image 20230531211750.png]]


### 4. Response

Various Response messages covered above in other message types



## Ports used

SNMP Agents use *UDP Port 161*

SNMP Managers use *UDP Port 162*



## Configuration

![[Pasted image 20230531212119.png]]


## Wireshark Capture

![[Pasted image 20230531212134.png]]
^**Note**
- version = 2c
- note `community` is visible
	- This is because in SNMPv1 and SNMPv2c, there is no encryption. The community and messages are sent in plain-text.
	- This is not secure, as the packets can easily be captured and read
	- That is why SNMPv3 is preferred



---

# Quiz

![[Pasted image 20230531213958.png]]
(d, g)


![[Pasted image 20230531214032.png]]
(a, b) - Since these are sent from Managed devices to the NMS


![[Pasted image 20230531214106.png]]
(d)


![[Pasted image 20230531214128.png]]
(c)

![[Pasted image 20230531214146.png]]
(d)


![[Pasted image 20230531214211.png]]
(c, e)
