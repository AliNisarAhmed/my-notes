# Blocking an IP Address

![b4599c267397dff472c700f1ccb67958.png](../../images/b4599c267397dff472c700f1ccb67958.png)

In the above diagram, blocking can happen at either NACL or the Software Firewall in EC2
- SG do not have DENY rules thus no blocking at SG level


![988ad1c727bc9c146c152d2411608ead.png](../../images/988ad1c727bc9c146c152d2411608ead.png)

With an ALB, we can only block at NACL level
- Since instances are behind ALB and they do not directly see client ip
- We can block with Ec2 firewall as well 


![0fae9fb859f828b918301f58a7f16f1d.png](../../images/0fae9fb859f828b918301f58a7f16f1d.png)


NLB does not have Security Groups, thus traffic goes through them

Again, blocking at NACL level


![127c0a6cecefa59fde0cd7d9f669bb50.png](../../images/127c0a6cecefa59fde0cd7d9f669bb50.png)


ALB + WAF can be used to block IPs


![95ab610e6b0b7c8b8d8f994910956ebe.png](../../images/95ab610e6b0b7c8b8d8f994910956ebe.png)


Withh ALB + CloudFront, we must use either Geo-Restriction or WAF, NACL is no longer able to block IPs since it only sees cloudFront IP rather than client's