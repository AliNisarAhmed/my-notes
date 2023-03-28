# VPC

![ba99bdbc7ca2fd4c0d1f341d7413758f.png](../../images/ba99bdbc7ca2fd4c0d1f341d7413758f.png)


![1da6fe85ab0d13b839b3a7a80d4ec44e.png](../../images/1da6fe85ab0d13b839b3a7a80d4ec44e.png)

![72c1b8473fd1972a012cc00a95b0c5dd.png](../../images/72c1b8473fd1972a012cc00a95b0c5dd.png)

![f1bf95fb86ead23639ea31df52c194ff.png](../../images/f1bf95fb86ead23639ea31df52c194ff.png)




### CIDR

![6982ad5736fb33a6d27b382326f94f95.png](../../images/6982ad5736fb33a6d27b382326f94f95.png)

![0c9086433741dd34ce635acb37ed2832.png](../../images/0c9086433741dd34ce635acb37ed2832.png)



### Public vs Private IPs

The IANA (Internet Assigned Number Authority) established certain blocks of IPv4 address for use of private (LAN) and public addresses

Private IP can only be certain values:
- 10.0.0.0/8 (i.e. up to 10.255.255.255) -> in big networks
- 172.16.0.0/12 (i.e up to 172.31.255.255) -> AWS default VPC in that range
- 192.168.0.0/16 (up to 192.168.255.255) -> e.g. home networks

All the remaining IP addresses are public


## VPC

### Default VPC

All new AWS accounts have a default VPC

New instances are launched in default VPC if none specified

Default VPC has internet connectivity 

All EC2 inside default VPC have public IPv4 addresses
- we also get a public and private IPv4 DNS name


### VPC in AWS

Max 5 VPC allowed per region (soft limit)

Max CIDR per VPC is 5, for each CIDR:
- Min size is /28 (i.e. 16 IP addresses)
- Max size is /16 (65536 IP addresses)

Because VPC is private, only the Private IPv4 ranges are allowed:
- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)

Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)

### AWS Reserved IP addresses

AWS reserves 5 IP addresses (first 4 and last)

for a CIDR 10.0.0.0/24, these will be
1. 10.0.0.0 - Network Address
2. 10.0.0.1 - reserved by AWS for VPC router
3. 10.0.0.2 - reserved by AWS for mapping to Amazon-provided DNS
4. 10.0.0.3 - reserved by AWS for future use
5. 10.0.0.255 - Network Broadcast address

**Exam Tip**: if you need 29 IP addresses for EC2 instances:
• You can’t choose a subnet of size /27 (32 IP addresses, 32 – 5 = 27 < 29)
• You need to choose a subnet of size /26 (64 IP addresses, 64 – 5 = 59 > 29)


—

## Internet Gateway

Allows resources in a VPC to connect to internet

1 VPC ↔ 1 IGW

**NOTE:** IGW on their own do not allow internet access - their Route Tables must be edited

![bb4eb0ec1b0f0378d003825ab2d19450.png](../../images/bb4eb0ec1b0f0378d003825ab2d19450.png)


## Bastion Hosts

We use Bastion Hosts to SSH into our private instances

We can declare any instance in a public subnet to be a Bastion Host

For Communication
- Bastion Host SG must allow inbound from internet on port 22
    - preferrably from restricted CIDR e.g. public CIDR of your company
- SG of private EC2 instances must allow the SG of Bastion Host 
    - OR the private IP of BH
    - but former is preferred

![672fb70096bd86a5f2aac3a74f346100.png](../../images/672fb70096bd86a5f2aac3a74f346100.png)


## [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)

AWS managed NAT
- A NAT gateway is a Network Address Translation (NAT) service. You can use a NAT gateway so that instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.

NATGW is created in a specific AZ

Uses an Elastic IP

Can't be used by EC2 instance in the same subnet (only from other subnets)

Requires an IGW (Private Subnet => NATGW => IGW)

5Gbps of bandwidth with auto scaling up to 45 Gbps

No SGs to manage

We must create multiple NATGWs in multiple AZs for fault-tolerance

![0e7e52bd18728c046fc717e3beb79750.png](../../images/0e7e52bd18728c046fc717e3beb79750.png)

![dadc0ccb7bdeafdcc6a231fb0615b6a9.png](../../images/dadc0ccb7bdeafdcc6a231fb0615b6a9.png)

in addition to above:

port forwarding | not supported | Manually customize the config to support pf


https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html


## NACL (N/w Access Control List)

NACLs are like firewalls for **subnets**

1 NACL per subnet, new subnets are assigend the default NACL

You define NACL Rules:
- Rules have a number (1-32766), higher precedence with a lower number
- First rule match will drive the decision
- Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP
address will be allowed because 100 has a higher precedence over 200
- The last rule is an asterisk (*) and denies a request in case of no rule match
- AWS recommends adding rules by increment of 100

Newly created NACLs will deny everything

NACL are a great way of blocking a specific IP address at the subnet level

![bdaefc4c85c0caf64796b94cff51720b.png](../../images/bdaefc4c85c0caf64796b94cff51720b.png)


![193251edec89ff77cc57ae562d102f9c.png](../../images/193251edec89ff77cc57ae562d102f9c.png)


![4d27d2f4d112d8c33966c384f74cab69.png](../../images/4d27d2f4d112d8c33966c384f74cab69.png)


## VPC Peering

![f3d370b710dbecc0fa011ffc5c46ebb7.png](../../images/f3d370b710dbecc0fa011ffc5c46ebb7.png)

Good to know:
- VPC Peering can happen between VPCs in **different AWS regions**
- You can reference an SG in the peered VPC (works across accounts but same region)


## VPC Endpoints (AWS PrivateLink)

VPC Endpoints (powered by AWS PrivateLink) allows you to connect to AWS services using a private network instead of public internet

They remove the need for IGW, NATGW etc to access AWS services

In case of issues:
• Check DNS Setting Resolution in your VPC
• Check Route Tables

![268384d67c7dc56ee57a81fc8fc332d9.png](../../images/268384d67c7dc56ee57a81fc8fc332d9.png)


### Interface vs Gateway Endpoints

![485507fc5943fa4a29f9c217460d15e9.png](../../images/485507fc5943fa4a29f9c217460d15e9.png)

![2676fc1aeeaaa925bd3df71b5ac97d78.png](../../images/2676fc1aeeaaa925bd3df71b5ac97d78.png)

![c48aa8ce72b5fc7030d64e4a744c8c84.png](../../images/c48aa8ce72b5fc7030d64e4a744c8c84.png)


## VPC Flow Logs

![24e9dbf1a3211c118cfef0e88b8a42e2.png](../../images/24e9dbf1a3211c118cfef0e88b8a42e2.png)


![68bf6be83ad08b3ba23000f620b69249.png](../../images/68bf6be83ad08b3ba23000f620b69249.png)



## AWS Site-to-Site VPN

![6b147e5cebf0f760a9aaed1cc749921d.png](../../images/6b147e5cebf0f760a9aaed1cc749921d.png)


![b9b12be114fd5130d95db295b381efc7.png](../../images/b9b12be114fd5130d95db295b381efc7.png)

![87456847d6f1d132f723450090595e5d.png](../../images/87456847d6f1d132f723450090595e5d.png)


## AWS VPN CloudHub

![0655d171c2a57763389f24097166a827.png](../../images/0655d171c2a57763389f24097166a827.png)


## Direct Connect (DX)

![5cfe157c0d6e0a3992b92a98388a3853.png](../../images/5cfe157c0d6e0a3992b92a98388a3853.png)

![edcbac04765a2c701c953b9fa52edab9.png](../../images/edcbac04765a2c701c953b9fa52edab9.png)


## Direct Connect Gateway

![7d4048389e17c4dcfe1b26cff6ac96a7.png](../../images/7d4048389e17c4dcfe1b26cff6ac96a7.png)


### Connection Types

#### Dedicated Connections

1Gbps, 10 Gbps and 100Gbps

Physical ethernet port dedicated to a customer
• Request made to AWS first, then completed by AWS Direct Connect Partners

#### Hosted Connections

50Mbps, 500 Mbps, to 10 Gbps

• Connection requests are made via AWS Direct Connect Partners
• Capacity can be added or removed on demand
• 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners


**Note** - Lead times are often longer than 1 month to establish a new connection

![d21cc7e1e3eb2622168287d8ae225d24.png](../../images/d21cc7e1e3eb2622168287d8ae225d24.png)


![74662b2b03752a1b5f68143c67099983.png](../../images/74662b2b03752a1b5f68143c67099983.png)



## Transit Gateway

![21b31ce344c106a4d83c9ebafbc19df1.png](../../images/21b31ce344c106a4d83c9ebafbc19df1.png)



## VPC Traffic Mirroring

![7d5fbbf208b954ebed0ce5536432ac41.png](../../images/7d5fbbf208b954ebed0ce5536432ac41.png)


## Egress-only Internet Gateway

![4421413b674514fb27dd5040a35151ff.png](../../images/4421413b674514fb27dd5040a35151ff.png)


## AWS Network Firewall

![0c81949442223c053f5e324c7573f004.png](../../images/0c81949442223c053f5e324c7573f004.png)



# Networking Costs

![25ad9523ebfd7f986c5ae59b4ae75ebc.png](../../images/25ad9523ebfd7f986c5ae59b4ae75ebc.png)

![e8ae5e265a08c448e19dd0271c67b81c.png](../../images/e8ae5e265a08c448e19dd0271c67b81c.png)

![7a2bd5b3f36bd9c8d6c67b393f56529e.png](../../images/7a2bd5b3f36bd9c8d6c67b393f56529e.png)

![ec569a68f8ebe0ca49c203c325d494a2.png](../../images/ec569a68f8ebe0ca49c203c325d494a2.png)

