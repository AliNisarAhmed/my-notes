
# Layer 2 Discovery Protocols


![[Pasted image 20230526224040.png]]


# CDP

![[Pasted image 20230526224116.png]]


## `show` Commands

![[Pasted image 20230526224251.png]]


![[Pasted image 20230526224408.png]]
^ Local Interface is the interface on the current device vs Port ID which shows the interface hostname of the other device
^ SW1 shows  `R S I` as it is a Layer 3 switch
^ Platform shows the model of the Cisco device (shows up in Packet tracer and real devices)

![[Pasted image 20230526224637.png]]

![[Pasted image 20230526224714.png]]


## Configuration

**NOTE**: CDP is enabled on all cisco devices on all their interfaces by default (unlike LLDP which must be manually enabled)

![[Pasted image 20230526224850.png]]


## Wireshark capture

![[Pasted image 20230526225905.png]]
^ No IP Packet


# LLDP

![[Pasted image 20230526224944.png]]

## Commands

![[Pasted image 20230526225430.png]]

![[Pasted image 20230526225505.png]]

![[Pasted image 20230526225552.png]]
^the time is not live (unlike CDP)

![[Pasted image 20230526225649.png]]
^ the timer is live in this command
^ note 2 capabilities - system and enabled capabilities

![[Pasted image 20230526225820.png]]

![[Pasted image 20230526225824.png]]


## Wireshark capture

![[Pasted image 20230526225943.png]]


---


# Quiz


![[Pasted image 20230526230032.png]]
(a, c)

![[Pasted image 20230526230133.png]]
(c, d)

![[Pasted image 20230526230158.png]]
(b) - B is for Bridge, R is for Router


![[Pasted image 20230526230231.png]]
(b, f)


![[Pasted image 20230526230334.png]]
(b)


![[Pasted image 20230526230357.png]]
(b, c, e, f)
^ to show a and e you have to use the command `show cdp neighbors detail`

