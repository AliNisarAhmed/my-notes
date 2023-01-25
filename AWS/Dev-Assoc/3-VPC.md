# VPC

![02869ae7471200ccc36ab8aee0b85ae0.png](../../../../dev/my-notes-work/images/02869ae7471200ccc36ab8aee0b85ae0.png)


A subnet within VPC can reside entirely within one AZ

but an AZ can have more that one subnet

Subnets can be public or private
- public for AWS EC2 public web servers
- private for web servers and DBs

![edcb5a54e3fbfafa829d0e4da4400bda.png](../../../../dev/my-notes-work/images/edcb5a54e3fbfafa829d0e4da4400bda.png)



## Anatomy of a VPC

An Amazon VPC consists of:
- CIDR Block
    - allowed block size is /16 to /28 netmask
    - AWS reserves 5 IP address from the above block - first four and last
- Subnets
- Route Table
    - controls the network traffic in your VPC through subnet routing
- DHCP Options
    - control the auto provisioning of IP addresses to EC2 instances
- NAT Devices
    - enable EC2 instances in a pvt subnet to connect to the public internet
    - prevents public internet from initiating connections with our pvt EC2 instances
- Network ACLs
    - protect at subnet level
    - one subnet -> one Network Access Control List
- Security Groups
    - attached to EC2
    - only have ALLOW rules
- Different types of Gateways
    - Internet Gateway
    - Customer Gateway
        - on-prem
    - Virtual Private Gateway
    - Carrier Gateway
    - Egress-only Internet Gateway
        - IPV6
        

![2aa780dbf478ea7ca634488d10d210d2.png](../../../../dev/my-notes-work/images/2aa780dbf478ea7ca634488d10d210d2.png)


S3, Lambda, Dynamo DB cannot be placed within a VPC, however, a VPC can connect to these services privately using VPC Endpoint

![7fb826e2198cfb5b1b2b31fb5d1eb6f3.png](../../../../dev/my-notes-work/images/7fb826e2198cfb5b1b2b31fb5d1eb6f3.png)


A **Public subnet** is a subnet that has a route to an Internet gateway in its route table. This links the VPC to the Internet as well as other publicly accessible AWS services such as Amazon S3 and Amazon DynamoDB.

A **Private subnet** is one that has a route table with no connection to an Internet gateway. Resources inside a private subnet do not allow inbound traffic from the internet.

![58ce709862c607ade02eb882d8f6bcaf.png](../../../../dev/my-notes-work/images/58ce709862c607ade02eb882d8f6bcaf.png)

The following are possible sources or destinations for a security group:

* A single IP address ( **/32** prefix length for IPv4 or **/128** prefix length for Ipv6)
* A range of IP addresses in CIDR block notation (ex: **1.2.3.4/24**)
* A security group ID (ex: **sg-0222278bcc3e231e9**)
* A prefix list ID (ex: **pl-63a5400a**)

Security groups implicitly deny traffic. No traffic passes through unless an inbound and outbound rule is specified. That being said, you can only create **ALLOW** rules; it’s not possible to create **DENY** rules.


VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to Amazon CloudWatch Logs and Amazon S3. After you’ve created a flow log, you can retrieve and view its data in the chosen destination.
