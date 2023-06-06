
# SysLog

![[Pasted image 20230602210134.png]]


## Message Format

![[Pasted image 20230602210203.png]]
^
`seq` is optional
`timestamp` is also optional
`facility`: example: ospf
`severity`: 8 severity levels


## Severity Levels

![[Pasted image 20230602210357.png]]

**Note**: Notice is also known as Notification

The above definitions vary between vendors

As per RFC 5424 (Syslog Protocol RFC)
> because severities are very subjective, a relay or collector should not assume that all originators have the same definition of severity 


**Mnemonic to remember** these levels:
"Every Awesome Cisco Engineer Will Need Icecream Daily"



## Message examples

![[Pasted image 20230602213447.png]]


## Syslog Logging locations


![[Pasted image 20230602213618.png]]


## Syslog vs SNMP

![[Pasted image 20230602215924.png]]


## Configuration (optional section)

![[Pasted image 20230602215129.png]]

![[Pasted image 20230602215304.png]]

![[Pasted image 20230602215424.png]]

![[Pasted image 20230602215715.png]]

![[Pasted image 20230602215705.png]]


---

# Quiz

![[Pasted image 20230602220234.png]]
(c)


![[Pasted image 20230602220309.png]]
(b)

![[Pasted image 20230602220332.png]]
(b, c)

![[Pasted image 20230602220711.png]]
(c)

![[Pasted image 20230602220733.png]]
(a, d)
