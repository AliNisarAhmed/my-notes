

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