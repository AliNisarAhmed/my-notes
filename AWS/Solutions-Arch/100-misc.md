
3. HIPAA Complaince
    - is **a living culture that health care organizations must implement within their business in order to protect the privacy, security, and integrity of protected health information**.e
7. ECS launch type pricing
    - EC2 type 
        - is free service, we pay for resources (EC2 instances) we use, and the amount of time the tasks are running on them
    - Fargate type 
        - is a pay-as-you-go service, and you only pay for the resources you use. 
        - The cost depends on the number of tasks you are running, the amount of time the tasks are running and the amount of memory and CPU your tasks require
13. NLB traffic routing (how?)
    - if you specify targets using an instance ID, traffic is routed to instances using primary private IP address specified in the primary ENI for the instance
    - If you specify target using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces. This enables multiple applications on an instance to use the same port.
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
31. S3 transition minimum period
    - The minimum storage duration is 30 days before you can transition objects from S3 Standard to S3 One Zone-IA or S3 Standard-IA, so both these options are added as distractors.
32. VPC Sharing vs Peering?
    - VPC sharing (part of Resource Access Manager) allows multiple AWS accounts to create their application resources such as EC2 instances, RDS databases, Redshift clusters, and Lambda functions, into shared and centrally-managed Amazon Virtual Private Clouds (VPCs). To set this up, the account that owns the VPC (owner) shares one or more subnets with other accounts (participants) that belong to the same organization from AWS Organizations. After a subnet is shared, the participants can view, create, modify, and delete their application resources in the subnets shared with them. Participants cannot view, modify, or delete resources that belong to other participants or the VPC owner.
    - You can share Amazon VPCs to leverage the implicit routing within a VPC for applications that require a high degree of interconnectivity and are within the same trust boundaries. This reduces the number of VPCs that you create and manage while using separate accounts for billing and access control.
34. public VIF?
    - AWS DataSync is an online data transfer service that simplifies, automates, and accelerates copying large amounts of data between on-premises storage systems and AWS Storage services, as well as between AWS Storage services.
    - DX VIFs: https://repost.aws/knowledge-center/public-private-interface-dx
37. CloudFormation Stacksets vs Recycle Bin?
    - Recycle Bin
        - a data recovery new feature
        - enables one to restore accidentally deleted Amazon EBS snapshots and EBS-backed AMIs
        - If the resources are deleted, they are retained in the Recycle bin for a time period that you specify before being permanently deleted
    - Cloudformation stacksets
        - extends the capability of stacks by enabling you to create, update or delete stacks across multiple accounts and AWS regions with a single operation
43. CloudFront Signed Cookies
    - allow you to control access to multiple content files and you dont have to change your URL for each customer access and these dont expire (unlike S3 Presigned URLs)
44. S3 - setting permission for website access
    - When you configure a bucket as a static website, if you want your website to be public, you can grant public read access. 
    - To make your bucket publicly readable, you must disable block public access settings for the bucket and write a bucket policy that grants public read access. 
    - If your bucket contains objects that are not owned by the bucket owner, you might also need to add an object access control list (ACL) that grants everyone read access.
    - Amazon recommends disabling Object ACLs by using Object Ownership thus defaulting to normal S3 permissions
45. Cloudwatch metrics aggregation
    - Instance unified cloudwatch agent and use aggregation_dimensions in agent config file to aggregate metics for all instances
47. S3 vs S3 TA pricing
    - S3 itself has no transfer charges when data is transferred from the internet
    - with S3TA, you only pay for transfers that are successfully accelerated
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
58. Is EFS multi-region by default, if not, how to make it so?
    - use VPC peering for inter-region sharing
59. EFS vs EBS gp2 vs S3 Standard price comparison
    - EFS         = 0.3 per GB per month
    - EBS gp2     = 0.1 per GB per month
    - S3 Standard = 0.023 per GB per month
60. S3 multi-account upload
    - By default, an S3 object is owned by the AWS account that uploaded it. This is true even when the bucket is owned by another account
    - To get access to the data files, an AWS Identity and Access Management (IAM) role with cross-account permissions must run the UNLOAD command again.
    1.  From the account of the S3 bucket, create an IAM role (Bucket Role) with permissions to the bucket.
    2.  From the account of the Amazon Redshift cluster, create another IAM role (Cluster Role) with permissions to assume the Bucket Role.
    3.  Update the Bucket Role to grant bucket access and create a trust relationship with the Cluster Role.
    4.  From the Amazon Redshift cluster, run the UNLOAD command using the Cluster Role and Bucket Role.
63. EFS IA
    - Amazon EFS Infrequent Access (EFS IA) is a storage class that provides price/performance that is cost-optimized for files, not accessed every day, with storage prices up to 92% lower compared to Amazon EFS Standard
67. Permission boundary on Groups?
    - AWS supports permissions boundaries for IAM entities (users or roles, NOT groups). A permissions boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity. An entity's permissions boundary allows it to perform only the actions that are allowed by both its identity-based policies and its permissions boundaries.
69. EC2 default termination policy
    - https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-termination-policies.html
71. Resource Access Manager (VPC/subnet sharing?)
72. Shared Services VPC
    - Consider an organization that has built a hub-and-spoke network with AWS Transit Gateway. VPCs have been provisioned into multiple AWS accounts, perhaps to facilitate network isolation or to enable delegated network administration. When deploying distributed architectures such as this, a popular approach is to build a "shared services VPC, which provides access to services required by workloads in each of the VPCs. This might include directory services or VPC endpoints. Sharing resources from a central location instead of building them in each VPC may reduce administrative overhead and cost.
73. Dedicated instances vs Dedicated hosts
    - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html#dedicated-hosts-dedicated-instances
74. Private Hosted Zones - DNS hostnames and DNS Resolutio
75. Transfer data between EFS in 2 different regions
76. Kinesis Video Streams
77. Aurora Parllel Query
78. CloudFormation Mappings vs Parameters
79. AQUA for redshift