##### Resources

- subnettingquestions.com
- subnetting.org
- subnettingpractice.com

##### Binary form of Subnet Masks

0000_0000 = 0    = /24
1000_0000 = 128 =  /25
1100_0000  = 192  = /26
1110_0000   = 224 = /27
1111_0000   = 240  = /28
1111_1000    = 248  = /29
1111_1100    = 252  = /30
1111_1110     = 254  = /31
1111_1111      = 255  = /32

 
##### Subnetting on 4th octet


Allocated IP = Class C: 200.15.10.0/24
We want /28 subnets

Available Subnets = 2 ^ (subnet_bits)
	where
		subnet_bits = Given subnet - Class Default Subnet
		subnet_bits = /28 - /24 = 4
		Available subnets = 2^4 = 16

Available hosts = 2 ^ (host_bits) - 2
	where
		host_bits = 32 - Given subnet
		host_bits = 32 - 28 = 4
		Available hosts = 2^4 - 2 = 14


---


Allocated = Class C: 200.15.10.0/24

Given Subnet = /31

Available networks = 2^(31 - 24) = 2 ^ 7 = 128 networks
Available hosts = 2 ^ (32 - 31) = 2
	- **NOTE**: we do not subtract 2 from hosts because for /31, network and broadcast addresses are not used
	- That is why /31 is used for point-to-point connections (connecting 2 computers together)
Vallid host addresses:
	- 200.15.10.0 to 200.15.10.1 (separated by 2)
	- 200.15.10.2 to 200.15.10.3
	- ....
	- 200.15.10.254 to 200.15.10.255


---


Allocated = Class C: 200.15.10.0/24
Given Subnet = /30

Available networks = 2^6 = 64
Available hosts = 2 ^ 2 - 2 = 2 per network
Valid host addresses:
- 200.15.10.1 to 200.15.10.2 (network = .0, broadcast = .3) (separated by 4)
- 200.15.0.5 to 200.15.10.6 (network = 0.4, broadcast = .7)
- ...
- 200.15.0.253 to 200.15.10.254 (network = 200.15.0.252, broadcast = 200.15.0.255)


---

Allocated = Class C: 200.15.10.0/24
Given Subnet = /29

Available networks = 2^5 = 32
Available hosts = 2^3 - 2 = 6 hosts per network
Valid host addresses:
- 200.15.10.1 to 200.15.10.6 (N = .0, B = .7)
- 200.15.10.9 to 200.15.10.14 (N = .8, B = .15)
- ...
- 200.15.10.249 to 200.15.10.254 (N = .248, B = .255)


---

Questions
- What are n/w address, broadcast address and valid host addresses for the IP 198.22.45.173/26?
- Subnet mask in dotted decimal?

- Subnet Mask: 255.255.255.192
-  Allocated

- network portion is first 26 bits
	- the first 3 octets remain the same
	- thus the network address is 198.22.45.10,000000 = 198.22.45.128
- Available hosts = 2^6 - 2 = 64 - 2 = 62
- Valid host addresses:
	- 198.22.45.129 to 198.22.45.190 (N = 198.22.45.128, B = 198.22.45.191)


**Other Way of Solution**

198.22.45.173 is initially a part of the 198.22.45.0/24 network.

Now, we are required to find the usable range where 198.22.45.173/26 belongs.

![](https://img-c.udemycdn.com/redactor/raw/2020-04-23_10-33-09-3fe601270ca5823d7d4a9934ce310e99.PNG)

  

From the big network 198.22.45.0, we will increment by 64 to determine its subnets.

subnet 1: 198.22.45.0/26

subnet 2: 198.22.45.64/26

subnet 3: 198.22.45.128/26 **(198.22.45.173/26 belongs to this subnet)**

subnet 4: 198.22.45.192/26

  

Now, we focus on **subnet 3** because that's where the required 198.22.45.173/26 belongs.

198.22.45.128 is the network address

198.22.45.191 is the broadcast address

198.22.45.128-198.22.45.191 is the subnet range

198.22.45.129-198.22.45.190 is the usable or valid range



---

Allocated: Class C 200.15.10.0/24

We have two offices in NewYork and Boston
- We want to subnet two Engineering departments with 28 hosts
- Followed by Sales department 1 with 14 Hosts
- Followed by Sales Dept 2 with 7 hosts
- Followed by link b/w the routers in 2 cities

Solution:
- use /27 as it supports 30 hosts (2^5 - 2)
- Engineering Dept 1
	- N/w address:            200.15.10.0/27
	- Broadcast address: 200.15.10.31
	- Hosts
		- 200.15.10.1 to 200.15.10.30
		- Use the first host for router interface: 200.15.10.1
- Engineering Dept 2
	- N/w address:            200.15.10.32/27
	- Broadcast address: 200.15.10.63
	- Hosts
		- 200.15.10.33 to 200.15.10.62
		- Use the first host for router interface: 200.15.10.33
  - Sales Dept 1
	  - use /28 (2^4 - 2) = 14
	  - N/w address:            200.15.10.64/28
	  - Broadcast address: 200.15.10.79
	  - Hosts
		  - 200.15.10.65 to 200.15.10.78
		  - n/w interface address: 200.15.10.65
  - Sales Dept 2
	  - use /28
	  - N/w address:             200.15.10.80/28
	  - Broadcast address:  200.15.10.95
	  - Hosts
		  - 200.15.10.81 to 200.15.10.94
		  - n/w interface address: 200.15.10.81
  - Routers
	  - use /30 (/31 also supports two hosts, but that is seldom used)
	  - N/w address:              200.15.10.96/30
	  - Broadcast address:   200.15.10.99
	  - Hosts
		  - 200.15.10.97 to 200.15.10.98


---

Allocated: Class B: 135.15.0.0/16 
Target: 135.15.10.138/29

Number of networks: 2^(29-16) = 2^13 = 8192
Number of hosts = 2^(32 - 29) - 2 = 2^3 - 2 = 6 per network
N/w address: 135.15.10.136
- how to calculate:
	- 138 in binary: 1000 1010
	- Since 2^3 is the host number, we drop/zero the last 3 bits
	- Hence: 10001000 = 136 (128 + 0 + 0 + 0 + 8 + 0...)
Broadcast address: 135.15.10.143
Next nw address: 135.15.10.144
Valid host addresses: 135.15.10.137 to 135.15.10.142


---

Allocated Class A: 60.0.0.0/8

Question: If we apply subnet mask 255.255.255.240
- how many subnets do we have?
- how many hosts per subnet?

Solution:
- subnet mask is 255.255.255.1111_0000
- which is /28
- which means we have
	- subnets = 2^(28 - 8) = 2^20 = 1,048,576
	- hosts: 2^(32-28) - 2 = 14


---

Allocated: Class A: 60.0.0.0/8
Target: 60.15.10.75/28

Question: 
- n/w address
- broadcast address
- range of valid IP addresses

Solution:
- n/w address = 60.15.10.64/28
	- 75 in binary: 01001011
	- host number: 2^4, zero that last 4 bits
	- 01000000 = 64
- Broadcast address = 60.15.10.79
- next network address = 60.15.10.80
- valid hosts
	- 60.15.10.65 to 60.15.10.78



---

Allocated: Class A 60.0.0.0/8

Question: subnet into /19
- how many networks
- how many hosts?

Solution:
- networks = 2^(19 - 8) = 2^11 = 2048
- hosts = 2^(32 - 19) - 2 = 2^13 - 2 = 8190


---

Q: for IP 172.19.216.50 Mask=255.255.255.240
- what is the n/w addr
- broadcast addr
- valid hosts

Solution:
- n/w addr: 172.19.216.48
	- host number: 256 - 240 = 16 or 2^4
	- starting with 172.19.216.0 , keep adding 16 tilll we reach <50
	- hence: 172.19.216.48
- broadcast addr:
	- 172.19.216.63
- next n/w addr
	- 172.19.216.64
- valid hosts:
	- 172.19.216.49 to 62

---

#### Subnetting on 3rd octet


Allocated: Class A: 60.0.0.0/8
Given: 60.15.10.75/19
Question:
- n/w address
- Broadcast address
- next network address
- valid hosts

Solutions:
- n/w address = 60.15.0.0/19
	- Given IP in binary = 60.15.0000_1010.0100_1011
	- host num: 32-19 = 13, hence zero last 13
	- 60.15.0.0
- Broadcast address: 60.15.31.255
	- Subnet mask is 255.255.224.0
	- subtract value in subnetted octet = 256 - 224 = 32 (number to increment for next n/w address)
- next n/w address
	- 60.15.32.0
- Valid hosts
	- 60.15.0.1 to 60.15.31.254


---

Question: subnet 134.65.0.0 to 6 different networks
- what subnet mask to use

Solution:
- from 134.x.0.0 we know its a class B
- default subnet mask is /16
- to get 6 networks, we can use /19 or 255.255.224.0
- 8190 hosts in each n/w (2^13 -2)
- next network addr: 
	- 134.65.32.0 
	- 134.65.64.0
	- (256 - 224) = 32


---

Q: IP = 172.19.216.50/23
- n/w addr
- b/c addr
- valid hosts

Solution:
- Since its /23 we are subnetting on 3rd octet
	- another way to see this: Mask = /23 = 255.255.254.0
- n/w addr: 172.19.216.0
- next network addr: 172.19.218.0 (+2 on 3rd octet, since 256 - 254 = 2)
- broadcast: 179.19.217.255
- valid hosts
	- 172.18.216.1 to 172.18.217.254


---

Given: 192.168.1.0/24

Divide for
1. Tokyo LAN A = 110 hosts
2. Toronto LAN B = 45 hots
3. TN LAN A = 29 hosts
4. Tokyo LAN B = 8 hosts
5. point-to-point connection

Solution:

Tokyo LAN A = 110 hosts
- use /25 prefix length = 255.255.128.0
- n/w addr = 192.168.1.0
- b/c addr  = 192.168.1.127
- 1st IP = 192.168.1.1
- Last IP = 192.168.1.126
- Total hosts = 128 - 2 = 126

TN LAN B = 45 hosts
- /26
- n/w addr = 192.168.1.128/26 (starts right after Tokyo LAN B)
- b/c addr = 192.168.1.191
- 1st IP = 192.168.1.129/26
- Last IP = 192.168.1.190/26
- Total hosts = 64 - 2 = 62

TN LAN A = 29 hosts
- prefix length = /27
- n/w addr = 192.168.1.192
- b/c addr = 192.168.1.223
- 1st IP = 192.168.1.193
- Last IP = 192.168.1.222
- Total Hosts = 32 - 2 = 30

Tokyo LAN B = 8 hosts
- prefix length = /28
- n/w addr = 192.168.1.224
- b/c addr = 192.168.1.239
- 1st IP = 192.168.1.225
- Last IP = 192.168.1.238
- Total Hosts = 14

Point-to-Point
- prefix length = /30
- n/w addr = 192.168.1.240
- b/c addr = 192.168.1.243
- 1st IP = 192.168.1.241
- Last IP = 192.168.1.242
- Total Hosts = 4 - 2 = 2


---

Given: 192.168.5.0/24

Targets:
1. LAN2 = 64 hosts
2. LAN1 = 45 hosts
3. LAN3 = 14 hosts
4. LAN4 = 9 hosts
5. point-to-point b/w two Routers

Solution:

LAN2 = 64 hosts
- prefix length = /25 = 255.255.255.128
- n/w addr = 192.168.5.0
- b/c addr = 192.168.5.127
- 1st IP = 192.168.5.1
- Last IP = 192.168.5.126
- Total Hosts = 128 - 2 = 126

LAN1 = 45 hosts
- prefix length = /26 = 255.255.255.192
- n/w addr = 192.168.5.128
- b/c addr = 192.168.5.191
- 1st IP = 192.168.5.129
- Last IP = 192.168.5.190
- Total Hosts = 62

LAN3 = 14 hosts
- prefix length = /28 = 255.255.255.240
- n/w addr = 192.168.5.192
- b/c addr = 192.168.5.207
- 1st IP = 192.168.5.193
- Last IP = 192.168.5.206
- Total Hosts = 14

LAN 4 = 9 hosts
- prefix length = /28
- n/w addr = 192.168.5.208
- b/w addr = 192.168.5.223
- 1st IP = 192.168.5.209
- Last IP = 192.168.5.222
- Total Hosts = 14

P2P
- prefix length = /30
- n/w addr = 192.168.5.224
- b/c addr = 192.168.5.227
- 1st IP = 192.168.5.225
- Last IP = 192.168.5.226
- Total Hosts = 2