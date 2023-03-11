Remember 5 pillars of good architecture:
1. Costs
2. Performance
3. Reliability
4. Security
5. Operational Excellence

# Desiging WhatsTheTime.com


Scenario:
- allows people to know what time it is
- We don't need a DB
- We want to start small and can accept downtime
- We eventually want to scale fully, no downtime


## Solution

1. Start

Single t2.micro instance with Elastic IP

![b6027babac56cf46325521aac830e608.png](../../images/b6027babac56cf46325521aac830e608.png)

First step we can do is to upgrade to a Bigger instance (M5), but there is a downtime while upgrading

![d1c057db4e36ec40ecaeb7299fe93a0d.png](../../images/d1c057db4e36ec40ecaeb7299fe93a0d.png)    
  
  ![ef84d8ca88fe24ef9c3f64e63e96af90.png](../../images/ef84d8ca88fe24ef9c3f64e63e96af90.png)
  
  
  ![dd4c2df5352d5b59464a28a4e10fba6e.png](../../images/dd4c2df5352d5b59464a28a4e10fba6e.png)
  
  
 
 Instead of allowing users to connect directly to instances and risk breaking for some of them when an instance is down, we can front our instances by an ALB. 
 
 **Note**: When using an ALB we cannot use an A record, rather we have to use an Alias record to point our URL to ALB's DNS name
 
 ![83014cab3a168c0e23a52e66c2b568f9.png](../../images/83014cab3a168c0e23a52e66c2b568f9.png)
 
 
 In the above diagram, 1 problem is that we have to add/remove instances manually. 
 
 Solution is to add an Auto-scaling group
 
 ![45e992989fb7c0b6101dcdbff4525da3.png](../../images/45e992989fb7c0b6101dcdbff4525da3.png)
 
 
 To achieve High Availability, we enable Multi AZ on the ALB and ASG
 
 ![3c51bb5455343f39704866a7d887c38a.png](../../images/3c51bb5455343f39704866a7d887c38a.png)
 
 
 We can also save on costs by using reserved instances
 
 ![8b2abef246d235c67ead16229dee5270.png](../../images/8b2abef246d235c67ead16229dee5270.png)
 