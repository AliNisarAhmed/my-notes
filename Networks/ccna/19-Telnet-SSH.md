## Console Port security

### Login

![[Pasted image 20230603204952.png]]
^Why is there a single console line? because there can only be a single console connection at once, you cannot have multiple people configuring the device at once, only 1 user can configure it at a time


### Login local

![[Pasted image 20230603205651.png]]


## Layer 2 Switch - Management IP

![[Pasted image 20230603210345.png]]
^ Since there are not Routing Tables on the switches, the last command is specialized for switches to allow remote devices outside of the SW's subnet to access it using the Router as a default gateway


# Telnet

![[Pasted image 20230603210820.png]]

## Configuration

![[Pasted image 20230603211623.png]]

![[Pasted image 20230603211646.png]]
^ because of access-list, R2 could not telnet-connect to PC1, only PC2 could
^ vty lines are displayed split into 5-11, just a quirk, nothing important


# SSH

![[Pasted image 20230603211825.png]]

SSH used *TCP Port 22*


## Configuration

![[Pasted image 20230603211846.png]]

![[Pasted image 20230603214850.png]]

![[Pasted image 20230603214822.png]]


### Summary

![[Pasted image 20230603214936.png]]


# Commands Summary

![[Pasted image 20230603214948.png]]

---


# Quiz

![[Pasted image 20230603215038.png]]
(a, e)


![[Pasted image 20230603215110.png]]
(c, d)

![[Pasted image 20230603215135.png]]
(b)


![[Pasted image 20230603215237.png]]
(b,  f)


![[Pasted image 20230603215330.png]]
(b) - PC1 is the client


![[Pasted image 20230603215425.png]]
(b)