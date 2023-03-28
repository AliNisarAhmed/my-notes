1. Cross account S3 requests - Lambda to cross account S3 request
    - AWS Lambda can assume an IAM Role in another AWS account to access resources (e.g. S3) and do tasks (starting/stopping instances)
    - Configure your Lambda function's execution role to allow the function to assume an IAM Role in another AWS account
    - Modify your cross-account IAM Role's trust policy to allow your Lambda function to assume the role
3. HIPAA Complaince
    - is **a living culture that health care organizations must implement within their business in order to protect the privacy, security, and integrity of protected health information**.
5. launch configuration vs launch template
    - Launch templates (LTs) are newer than LCs and provide more options to work with
    - AWS recommends you create ASG from LTs for latest features
    - LC
        - A _launch configuration_ is an instance configuration template that an Auto Scaling group uses to launch EC2 instances.
        - Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping.
        - You can specify your launch configuration with multiple Auto Scaling groups. 
        - However, you can only specify one launch configuration for an Auto Scaling group at a time, and you can't modify a launch configuration after you've created it. To change the launch configuration for an Auto Scaling group, you must create a launch configuration and then update your Auto Scaling group with it.
    - LT is similar
        - allows versioning
        - Allows to edit and update.
        - With versioning of launch templates, you can create a subset of the full set of parameters. Then, you can reuse it to create other versions of the same launch template.
        - Also LTs provide **more EC2 options** for you to configure, for example, dedicated hosting can be set only using a LT. Similarly, ability to use T2 unlimited burst credit option is only available in a LT.
7. ECS launch type pricing
    - EC2 type 
        - is free service, we pay for resources (EC2 instances) we use, and the amount of time the tasks are running on them
    - Fargate type 
        - is a pay-as-you-go service, and you only pay for the resources you use. 
        - The cost depends on the number of tasks you are running, the amount of time the tasks are running and the amount of memory and CPU your tasks require
8. Lambda functions and VPC
    - Lambda functions always operate from an AWS-owned VPC. By default, your function has the full ability to make network requests to any public internet address — this includes access to any of the public AWS APIs.
    - Once your function is VPC-enabled, all network traffic from your function is subject to the routing rules of your VPC/Subnet. If your function needs to interact with a public resource, you will need a route through a NAT gateway in a public subnet.
    - You should only enable your functions for VPC access when you need to interact with a private resource located in a private subnet. An RDS instance is a good example.
9. Copying S3 data into different regions
    - 2 options
        1. Set up S3 batch replication to copy objects across S3 buckets in different region using S3 Console
            - as opposed to S3 Live replication - which requires a support ticket to AWS
        3. Copy data from the source S3 bucket to destination bucket using the AWS S3 sync command
          
11. Kinesis Data Stream performance
    - Amazon Kinesis Data Streams (KDS) is a massively scalable and durable real-time data streaming service. KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events.
    - By default, the 2MB/second/shard output is shared between all of the applications consuming data from the stream. You should use enhanced fan-out if you have multiple consumers retrieving data from a stream in parallel. With enhanced fan-out developers can register stream consumers to use enhanced fan-out and receive their own 2MB/second pipe of read throughput per shard, and this throughput automatically scales with the number of shards in a stream.
13. NLB traffic routing (how?)
    - if you specify targets using an instance ID, traffic is routed to instances using primary private IP address specified in the primary ENI for the instance
    - If you specify target using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces. This enables multiple applications on an instance to use the same port.
14. S3 Consistency model
    - S3 delivers strong read-after-write consistency automatically, without changes to performance or availability, w/o sacrificing regional isolation for applications, and at no additional cost
    - all S3 GET, PUT, and LIST operations, as well as operations that change object tags, ACLs, or metadata, are strongly consistent. What you write is what you will read, and the results of a LIST will be an accurate reflection of what’s in the bucket.
16. Gaurd Duty data sources
    - Threat detection service
    - Multiple sources, notably CloudTrail events, VPC Flow Logs and DNS Logs
18. Direct Connect (read that section again)
    - AWS Direct Connect is a networking service that provides an alternative to using the internet to connect to AWS. Using AWS Direct Connect, data that would have previously been transported over the internet is delivered through a private network connection between your on-premises data center and AWS.
20. Data moved from AWS S3 to AWS Kinesis Data Streams, to be implemented in the least possible time
    - You can use AWS DMS for such data-processing requirements. AWS DMS lets you expand the existing application to stream data from Amazon S3 into Amazon Kinesis Data Streams for real-time analytics without writing and maintaining new code.
22. AWS Shield Advanced high costs
    - Consolidated billing
        -  If your organization has multiple AWS accounts, then you can subscribe multiple AWS Accounts to AWS Shield Advanced by individually enabling it on each account using the AWS Management Console or API. 
        -  You will pay the monthly fee once as long as the AWS accounts are all under a single consolidated billing, and you own all the AWS accounts and resources in those accounts.
    -  AWS Shield Advanced does offer protection to resources outside of AWS. This should not cause unexpected spike in billing costs.
    -  No Savings plan available in AWS Shield Advanced
24. Redshift interplay with historical S3 data (Redhsift COPY)
    - Amazon Redshift is a fully-managed petabyte-scale cloud-based data warehouse product designed for large scale data set storage and analysis.
    - Using Amazon Redshift Spectrum, you can efficiently query and retrieve structured and semistructured data from files in Amazon S3 without having to load the data into Amazon Redshift tables.
    - Amazon Redshift Spectrum resides on dedicated Amazon Redshift servers that are independent of your cluster. Redshift Spectrum pushes many compute-intensive tasks, such as predicate filtering and aggregation, down to the Redshift Spectrum layer. Thus, Redshift Spectrum queries use much less of your cluster's processing capacity than other queries.
    - Loading historical data into Redshift via COPY command or Glue ETL job would cost heavy for a one-time ad-hoc process. The same result can be achieved more cost-efficiently by using Redshift Spectrum. Therefore both these options to load historical data into Redshift are also incorrect for the given use-case.
26. RDS Custom for Oracle Multi-AZ option?
27. EC2 placement groups - Spread vs Cluster
    - A spread placement group can span multiple AZs in the same Region
    - You can have a max of 7 running instances per AZ per placement group
29. DNS vs Global Accelerator for Blue/Green Deployments
    - With AWS Global Accelerator, you can shift traffic gradually or all at once between the blue and the green environment and vice-versa without being subject to DNS caching on client devices and internet resolvers, traffic dials and endpoint weights changes are effective within seconds.
    -  AWS Global Accelerator is a network layer service that directs traffic to optimal endpoints over the AWS global network, this improves the availability and performance of your internet applications. It provides two static anycast IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your Application Load Balancers, Network Load Balancers, Elastic IP addresses or Amazon EC2 instances, in a single or in multiple AWS regions.
30. CloudFront vs Global Accelerator
    - CloudFront uses multiple sets of dynamically changing IP addresses while Global Accelerator will provide you a set of static IP addresses as a fixed entry point to your applications.
    - CloudFront pricing is mainly based on data transfer out and HTTP requests while Global Accelerator charges a fixed hourly fee and an incremental charge over your standard Data Transfer rates, also called a Data Transfer-Premium fee (DT-Premium).
    - CloudFront uses Edge Locations to cache content while Global Accelerator uses Edge Locations to find an optimal pathway to the nearest regional endpoint.
    - CloudFront is designed to handle HTTP protocol meanwhile Global Accelerator is best used for both HTTP and non-HTTP protocols such as TCP and UDP.
    - Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require static IP addresses or deterministic, fast regional failover.
31. S3 transition minimum period
    - The minimum storage duration is 30 days before you can transition objects from S3 Standard to S3 One Zone-IA or S3 Standard-IA, so both these options are added as distractors.
32. VPC Sharing vs Peering?
    - VPC sharing (part of Resource Access Manager) allows multiple AWS accounts to create their application resources such as EC2 instances, RDS databases, Redshift clusters, and Lambda functions, into shared and centrally-managed Amazon Virtual Private Clouds (VPCs). To set this up, the account that owns the VPC (owner) shares one or more subnets with other accounts (participants) that belong to the same organization from AWS Organizations. After a subnet is shared, the participants can view, create, modify, and delete their application resources in the subnets shared with them. Participants cannot view, modify, or delete resources that belong to other participants or the VPC owner.
    - You can share Amazon VPCs to leverage the implicit routing within a VPC for applications that require a high degree of interconnectivity and are within the same trust boundaries. This reduces the number of VPCs that you create and manage while using separate accounts for billing and access control.
34. public VIF?
    - AWS DataSync is an online data transfer service that simplifies, automates, and accelerates copying large amounts of data between on-premises storage systems and AWS Storage services, as well as between AWS Storage services.
35. Amazon Aurora vs Aurora Serverless
    - Aurora Serverless is the auto-scaling on-demand config version of Aurora
37. CloudFormation Stacksets vs Recycle Bin?
    - Recycle Bin
        - a data recovery new feature
        - enables one to restore accidentally deleted Amazon EBS snapshots and EBS-backed AMIs
        - If the resources are deleted, they are retained in the Recycle bin for a time period that you specify before being permanently deleted
    - Cloudformation stacksets
        - extends the capability of stacks by enabling you to create, update or delete stacks across multiple accounts and AWS regions with a single operation
39. Networking modes in Amazon ECS tasks Ec2 launch type
    - Host Mode
    - Bridge mode
    - None mode
    - AWSVPC mode
41. WAF Rules
    - Following are the rate-based rules that can be used
        1. Blanket rate-based rules
            - prevents source IP address from making excessive requests to entire app
        2. URI specific rate-based rule
            - prevents IP address from making excessive requests to a particular URI
            - These are useful if specific functions of the app, e.g. computationally expensive resources need to be protected
        3. IP reputation rate-based rule
            - Prevents well-known malicious IP addresses from making excessive requests to the application
    - All the above rules can be combined or can be used separately as per need 
42. AWS Route 53 health checks and failovers
    - can be active-active OR active-passive failover configuration
    - Active-passive can is used when primary resources are available most of the time, and secondary resources are only used if primary resources are not available
    - Active-Active: Use this failover configuration when you want all of your resources to be available the majority of the time. When a resource becomes unavailable, Route 53 can detect that it's unhealthy and stop including it when responding to queries.
43. CloudFront Signed Cookies
    - allow you to control access to multiple content files and you dont have to change your URL for each customer access and these dont expire (unlike S3 Presigned URLs)
44. S3 - setting permission for website access
    - When you configure a bucket as a static website, if you want your website to be public, you can grant public read access. 
    - To make your bucket publicly readable, you must disable block public access settings for the bucket and write a bucket policy that grants public read access. 
    - If your bucket contains objects that are not owned by the bucket owner, you might also need to add an object access control list (ACL) that grants everyone read access.
    - Amazon recommends disabling Object ACLs by using Object Ownership thus defaulting to normal S3 permissions
45. Cloudwatch metrics aggregation
    - Instance unified cloudwatch agent and use aggregation_dimensions in agent config file to aggregate metics for all instances
46. EC2 Hibernate vs Standby mode
    - Hibernate is not supported for an instance that is part of an ASG (ASG marks it as dead and terminates it)
    - Standby Mode (as opposed to InService Mode) can be used to perform software upgrade or troubleshooting
47. S3 vs S3 TA pricing
    - S3 itself has no transfer charges when data is transferred from the internet
    - with S3TA, you only pay for transfers that are successfully accelerated
49. SQS FIFO Batch Mode
    - By default, FIFO queues support up to 300 msgs/s
    - When you batch 10 msgs per operation (max value), FIFO queue can support up to 3000 msgs/sec
51. S3 Invalid lifecycle transitions
    - Following are the unsupported life cycle transitions for S3
        - Any storage class to S3 Standard
        - Any storage class to the Reduced Redundancy storage class
        - S3 Int. Tiering to S3 Standard-IA
        - S3 1Z-IA to S3 Standard-IA or S3 Int. Tiering
53. AWS Guard Duty data deletion 
    - Disable the service in general settings, which will delete all data
54. S3 Retention
    - Different versions of a single object can have different retention modes and periods
55. S3 Versioning cannot be disabled once enabled, it can only be suspended
56. AWS Aurora replicas priority tiers
    - Each read replica is associated with a priority tier (0-15)
    - highest priority (i.e. lowest number) is promoted in case of failover
    - if priority is same, higher size is promoted
    - if size is same, an arbitrary replica is promoted
58. Is EFS multi-region by default, if not, how to make it so?
    - use VPC peering for inter-region sharing
59. EFS vs EBS gp2 vs S3 Standard price comparison
    - EFS         = 0.3 per GB per month
    - EBS gp2     = 0.1 per GB per month
    - S3 Standard = 0.023 per GB per month