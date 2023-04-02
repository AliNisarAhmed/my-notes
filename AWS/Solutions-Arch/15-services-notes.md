# EC2
- user data
    - script that runs before launch of an instance
    - use it to customize the dynamic installation parts at boot time
- Spot instances
    - available for less than on-demand price
    - available as long as your max price > spot price
    - Spot instance requests
        - either 1-time or persistent
            - persistent requests are reopened after spot instance is interrupted
        - Cancelling a spot request does not terminate instances
            - first cancel request, then terminate
    - Spot blocks
        - spot instances with a defined duration (1-6 hours)
        - designed not to be interrupted
        - may be reclaimed in rare situations due to EC2 capacity needs
    - Spot fleets
        - spot instances + (optional) on-demand
        - will try to meet target capacity with price constraints
        - strategies
            - lowestPrice
            - diversified
            - capacityOptimized
        - allow us to auto request spot instances with lowest price
- Reserved Instances
    - up to 75% discount
    - Classes/Types
        - Standard
            - provides more discount than Convertible
        - Convertible
            - can be exchanges for other RIs of a different instance family
    - Both classes can be modified though
    - after expiry
        - billed at on-demand price
        - so terminate asap and sell if not in use
- Dedicated instances vs Dedicated Hosts
    - Dedicated Instances
        - run on dedicated h/w, which is locked down for you while the instance is up
        - if instance is restarted, may switch to a different dedicated h/w
        - different h/w for different customers, but instances from same account may share h/w
        - does not have control and visibility on how instances are placed
    - Dedicated hosts
        - the same h/w as long as we pay
        - that is why useful in server license cases
        - support BYOL (Bring your own License)
        - visibility and control
- Placement Groups
    - Cluster
        - low latency group/cluster within single AZ
        - for very low n/w latency apps
    - Spread
        - spreads instances across underlying h/w (max 7 instances per group per AZ)
        - highly available
    - Partition
        - spreads instances across many different partitions (on different racks)
        - scales to 100s of EC2 instances per group
        - useful for Big Data computations
    - Errors during starting instances
        - try restarting all instances, adding new ones and starting them all again
- EC2 hibernate
    - States
        - pending (Not billed)
        - running (Billed)
        - stopping (billed if going to hibernate)
        - stopped (not billed even if hibernate)
        - shutting-down (Not billed)
        - terminated (not billed, unless RI)
    - Hibernate
        - RAM < 150GB
        - EBS root volume, encrypted
        - cannot hibernate > 60 days
        - Hibernate is not supported for an instance that is part of an ASG (ASG marks it as dead and terminates it), as well as ECS
    - Standby Mode (as opposed to InService Mode) can be used to perform software upgrade or troubleshooting
        - instance remains part of ASG
    - Once the instance is launched not possible to enable Hibernate
        - relaunch the instance with hibernation enabled
- Instance limits per Region
    - On-Demand Instances
        - vCPU-based
    - RIs
        - max 20
    - Spot instances
        - per your dynamic Spot limit
    - Newer accounts may start with lower limits
    - To Increase limit, submit limit increase request form
- On-Demand Capacity Reservations 
    - enable you to reserve compute capacity for your Amazon EC2 instances in a specific Availability Zone for any duration. 
    - This gives you the ability to create and manage Capacity Reservations independently from the billing discounts offered by Savings Plans or Regional Reserved Instances.
    - can create any time, w/o 1 or 3 yr commitment
- Savings Plan
    - use Cost Explorer to get started
    - Compute
        - 66%
    - Instance Savings Plan
        - 72%
        - commit to use same instance family
    - SageMaker
        - fir consistent amount of usage for 1 or 3 yr term



## EBS
- Bound to an AZ
- replicated within its AZ
    - to prevent data loss due to h/w failure
- Supports live config changes
    - even in prod
    - can modify volume type, volume size, and IOPs
- provisioned capacity
- DeleteOnTermination 
    - true by default for root volume
    - use case: preserve root volume when instance terminated
    - cannot set it for running instance from AWS Console
        - must use CLI
- Snapshots
    - can copy across regions or AZ
    - Snapshot Archive
        - 75% cheaper
        - takes 24-72 hours for restore
    - Recycle Bin
        - rules to prevent accidental deletion
        - specify retention (1 day to 1 year)
    - Fast Snapshot Restore
    - can use DLM (Data Lifecycle Manager) to auto manage EBS snapshots
    - can configure region level auto-encryption for all new EBS volumes & snapshots
- Volume Types
    - General purpose SSD
    - High performance (io1/io2)
        - business critical apps
        - can multi-attach
    - st1 low cost hdd
- RAID
    - RAID 0 when i/o matters 
    - RAID 1 when Fault tolerance matters



## Instance Store
- backups and replication your responsibility
- gp2/gp3 io1/io2 can be boot volumes
- Instance store is reset when you stop, terminate or hibernate the instance



## EFS
- works with EC2 instances in multi-AZ
- scales Auto
- Perf mode
    - General Purpose
        - For Latency sensitive use cases like web serving environments
    - Max I/O
        - Highly parallelized applications and workloads, such as big data analysis, media processing, and genomic analysis, can benefit from this mode.
- Throughput modes
    - Bursting
        - high throughput for load spikes
        - AWS recommends it by default
        - however, for large amounts of data migration, switch to provisioned
    - Provisioned
        - for requirements > bursting
        - can increase or decrease based on load
        - change b/w modes require 24 hours at least
- Storage Tiers
    - Standard
    - EFS-IA
        - cost to retrieve
        - lower price to store
            - save up to 92%
        - enabled with Lifecycle policy
        - max 90 days wait possible before moving to EFS-IA from EFS
- Availability
    - Standard
        - Multi-AZ
    - One-Zone
        - great for dev


## FSx
- Lustre
    - for HPC
    - 100s GB/s, millions IOPs, sub-ms latencies
    - seamless integration with S3
        - can read S3 as a filesystem
        - can write back to S3
    - can be used with on-prem (VPN or DX)
    - Deployment options
        - Scratch FS
            - Temporary storage
            - High Burst
            - usage: short term processing, optimize cost
        - Persistent FS
            - long-term
            - data replication within same AZ
            - usage: long-term, sensitive data
- Windows File Server
    - SMB + NTFS protocols
    - can be mounted on linux ec2
    - supports Microsoft Distributed File System Namespaces
    - 10s of Gbps, millions of IOPs, 100s of PB
    - can be access from on-prem (VPN or DX)
    - can be configued to be Multi-AZ
    - Daily back up to S3
- NetApp ONTAP
    - NFS, SMB, iScsi
    - Point in time instantaneous cloning
- OpenZFS




—

# HA

## ELB
- Highly available across AZs
- can run in 1 region only at a time
- Types
    - ALB
        - Supported Protocols
            - HTTP
            - HTTPS
            - HTTP/2
            - WebSocket
            - gRPC
        - Dynamic port mapping feature for ECS
        - Target Groups
            - Private IPs
            - EC2
            - Lambda
            - ECS Tasks
        - Supports SNI for multiple certs for multiple endpoints
        - cannot assign Elastic IP to an ALB
            - provides a fixed DNS but no fixed IP, even EIP
    - NLB
        - TCP
        - TLS
        - UDP
        - HTTP/HTTPS health checks when pointed to ALB only
        - 1 static IP per AZ
        - supports Elastic IP
        - Supports SNI for multiple certs for multiple endpoints
        - vs ALB
            - ALB expose a fixed DNS
            - while NLB expose a fixed IP
    - GLB
        - Layer 3 (IP)
        - for managing fleet of 3rd party n/w appliances


## ASG
- Free
- ELB health checks must be specifically enabled
- Have a scaling cooldown of default 300 seconds
- Rebalancing
    - launches new instances before terminating old ones to maintain perf/avail.
- Scaling activity
    - creates new scaling activity for terminating the unhealth instance first
        - then terminates it
    - later, another scaling activity launched to replace the terminated instance
- Launch templates (LTs) are newer than Launch Configs and provide more options to work with
    - AWS recommends you create ASG from LTs for latest features
    - LC
        - A _launch configuration_ is an instance configuration template that an Auto Scaling group uses to launch EC2 instances.
        - Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping.
        - You can specify your launch configuration with multiple Auto Scaling groups. 
        - However, you can only specify one launch configuration for an Auto Scaling group at a time, and you can't modify a launch configuration after you've created it. 
        - To change the launch configuration for an Auto Scaling group, you must create a launch configuration and then update your Auto Scaling group with it.
        - instance placement tenancy
            - default value is null and is taken from VPC tenancy attribute
            - unless set on LC
    - LT is similar
        - allows versioning
        - Allows to edit and update.
        - With versioning of launch templates, you can create a subset of the full set of parameters. Then, you can reuse it to create other versions of the same launch template.
        - Also LTs provide **more EC2 options** for you to configure, for example, dedicated hosting can be set only using a LT. Similarly, ability to use T2 unlimited burst credit option is only available in a LT.
        - allows choosing different instance types
- Scaling Policies
    - Target Tracking
        - track a target and scale accordingly
        - e.g I want average ASG CPU to stay around 40%
    - Simple Scaling
        - require CW alarms
        - there is a cooldown period
    - Step scaling
        - require CW alarms
        - very similar to Simple, except no cooldown
        - can specify Instance warm-up time
    - Scheduled Actions
    - Predictive Scaling
        - assumes that ASG has homogenous instances with equal capacity 
- Termination Policies
    - default
    - AllocationStrategy
    - OldestLaunchTemplate
    - OldestLaunchConfiguration
    - ClosestToNextInstanceHour
    - NewestInstance
    - OldestInstance
- default Termination Policy
    - All same instances
        - same as below except step 1
    - Mixed instances
        - 1. maintain allocation strategy
        - old LC > old LT
        - closest to billing hour
        - random
- health checks
    - default are EC2 health checks only
    - recommended to configure ELB based checks
        - ASG considers an instance unhealthy
            - if it fails either EC2 or ELB health checks
- Lifecycle hooks
    - custom actions when instance launch or terminate
    - hooks
        - EC2_INSTANCE_LAUNCHING
        - Pending:Wait
        - Pending:Proceed
        - Terminating:Wait
        - Terminating:Proceed
- Warm pools
    - collection of pre-initialized ec2 instances sitting along with ASG
    - in scale-out, ASG will pick an instance from warm pool



—

# DB & Cache

## RDS
- Continuous backups (PITR)
    - 1 to 35 days of retention
    - for more do manual backup or use AWS Backup
    - AWS creates a storage volume snapshot once a day during backup window
        - also captures transaction logs and uploads to S3 every 5 minutes
- read replicas
    - up to 5 read replicas
    - recommended to be of same compute and storage resources
    - replication is async (eventual consistency)
    - replicas can be promoted to their own DB
        - apps must update connection string
    - no fees for same-region replication
- Multi AZ
    - mainly used for DR and not read-enhancement
    - read replicas can be set up as multi-AZ as well
    - no need to stop the DB to switch for 1AZ to MulAZ
    - DB engine level upgrade
        - upgrades primary & secondary at the same time
        - this causes downtime until the upgrade is completed
- Auto-scaling
    - must set Max Scaling Threshold
    - can enable from Console w/o migrating/replacing DB
    - no additional cost
- RDS vs RDS Custom
    - full admin access to underlying OS and DB
- Fail-over
    - AWS provisions and maintans a secondary DB instance in different AZ
        - sync replicated
        - auto failover to the standby
    - during failover, RDS flips CNAME to point at the standby (NOT IP addr)
- RDS Proxy
    - improve DB efficiency by pooling connections
    - must be accessed from VPC
- Event Notifications
    - notifications are about the DB instance (not for the data)
    - Near real-time (up to 5 minutes)
    - Send notification to SNS or subscribe on EventBridge
- supports TDE (Transparent Data Encryption) with Oracle and SQLServer
- Enhanced Monitoring
    - monitor OS of your DB instance in real-time
    - gathers its metrics from an agent on the instance
    - while CW gathers from hypervisor
        - that's why Enhanced Monitoring metrics are more accurate 
    - provide more accurate info on:
        - RDS child processes
        - OS processes
- IAM DB Auth
    - no need to use DB username/password
    - supported only for PostgreSQL & MySQL RDS
    - benefits
        - in transit SSL
        - centrally manages access to DBs
        - for EC2, can use profile creds of instance instead of pwd

—

## Aurora
- 5x performance MYSQL, 3x performance PostgreSQL
- Read Replicas
    - 15 possible
    - AWS Aurora replicas priority tiers
        - Each read replica is associated with a priority tier (0-15)
        - highest priority (i.e. lowest number) is promoted in case of failover
        - if priority is same, higher size is promoted
        - if size is same, an arbitrary replica is promoted
- High Availability Native
    - 6 copies across 3 AZs
    - Cross Region Replication Support
        - useful for DR
- 20% more cost than RDS
- Aurora DB Cluster
    - 15 replicas
    - Dedicated Endpoints
        - e.g. Reader endpoint, writer endpoint, custom endpoint
- Auto Fail-over
    - Replica in same or different AZ
        - flips CNAME to healthy replica
        - takes 30 seconds max
    - No Replica & no serverless
        - attempt to create a new DB instance in same AZ as original
        - best-effort basis, may not succeed
- Aurora Serverless
    - auto-scaling on-demand config version of Aurora
    - Pay per second, can be more cost-effective
- Aurora Multi-Master
    - use case: immediate failover for write node
- Aurora Global DB
    - 1 primary region (read/write)
    - up to 5 secondary regions (1 second replication lag)
    - Up to 16 read replicas per secondary region
    - promoting another region has RTO of <1 min
- Aurora Cloning
    - Faster than snapshot and restore
    - very fast and cost-effective
    - useful to create dev/staging db w/o impacting prod
- Security
    - use IAM Roles
    - use SG 
- Aurora ML
    - perform ML using SageMaker & Comprehend
- Aurora Parallel Query
    - faster analytical queries, w/o having to copy data to another system
    - can speed up 2x, while maintaing high throughput for OLTP workloads
- Aurora Replicas vs MySQL Replicas
    - number
        - 15 : 5
    - replication type
        - async (milliseconds) : async (seconds)
    - performance impact on primary
        - low : high
    - replication location
        - in-region : cross-region
    - act as a failover target
        - yes (no data loss) : yes (potentially minutes of data loss)
    - automated failover
        - yes : no
    - support for user defined replication delay
        - no : yes
    - support for different data or schema vs primary
        - no : yes





## ElastiCache
- Redis
    - Multi-AZ
    - Read-replicas
    - persistant
    - Sets and Sorted Sets
    - geospatial data support
- MemCacheD
    - no HA
    - non persistent
    - no backup
    - multi-threaded
    - sharding
- Can be used to improve performance of
    - read-heavy apps
        - social n/w
        - gaming
        - media sharing
        - q&a portals
    - compute-intensive workloads
        - e.g. recommendation engine



—

# Route 53
- Public Hosted Zones
    - records to specify how to route traffic on the internet
- Private Hosted Zones
    - reocrds to specify how to route traffic in a VPC
- TTL mandatory except Alias record
- Alias record
    - CANNOT set it for EC2 DNS name
- Routing Policies
    - Simple
    - Weighted
    - Failover
    - Latency Based
        - does not guarantee users from same geographic region
    - geolocation
        - Use cases: website localization, restrict content distribution, load balancing
    - multi-value answer
    - geo-proximity
        - based on how close users are
        - allows to vary traffic using bias value to expand or shrink size of geographic region
- Health Checks
    - for public resources
    - Calculated Health checks for multiple health checks
    - For Private Hosted Zones, use CW Metric and Alarm, since Route53 health checkers cannot access private
    - AWS Route 53 health checks and failovers
    - can be active-active OR active-passive failover configuration
        - Active-passive 
            - is used when primary resources are available most of the time, and secondary resources are only used if primary resources are not available
        - Active-Active
            - Use this failover configuration when you want all of your resources to be available the majority of the time. 
            - When a resource becomes unavailable, Route 53 can detect that it's unhealthy and stop including it when responding to queries.
            - use weighted routing policy
                - no primary or secondary resource in this case
- VPC <-> on-prem DNS queries
    - for DNS queries in VPC from on-prem
        - create inbound endpoint on Route53 Resolver
        - DNS resolvers on on-prem can forward DNS queries to Route53 resolver via this endpoint
    - for DNS queries in on-prem from VPC
        - create outbound endpoint on Route53 Resolver
        - Route53 resolver then can conditionally forward queries to resolver on on-prem via this endpoint

— 

# Elastic Beanstalk
- Web Server Tier
- Worker Tier
    - do not run web servers
    - uses SQS 



—

# S3
- Max object size = 5TB
- must use multi-part upload for > 5GB
- Static Web Hosting
    - S3 bucket name must be the same as custom registered Domain name
    - S3 web endpoints formats:
        - http://{bucket-name}.s3-website-{Region}.amazonaws.com
        - http://{bucket-name}.s3-website.{Region}.amazonaws.com
- Versioning
    - can be enabled at bucket level
- Replication
    - versioning must be enabled
    - Cross-Region replication (CRR)
        - compliance
        - lower latency access
        - replication across accounts
        - takes about 15 minutes
            - not the faster way to copy data
            - just use Transfer Acceleration with MPUpload
    - Same-Region replication (SRR)
        - log aggregation
        - live replication b/w prod and test
    - Buckets can be in different AWS accounts
    - async copying
    - can replicate existing object using Batch Replication
    - Copying S3 data into different regions
        - 2 options
            1. Set up S3 batch replication to copy objects across S3 buckets in different region using S3 Console
                - as opposed to S3 Live replication - which requires a support ticket to AWS
            3. Copy data from the source S3 bucket to destination bucket using the AWS S3 sync command
- Durability
    - 11 9s
- Availability
    - 4 9s
- Consistency model
    - S3 delivers strong read-after-write consistency automatically, without changes to performance or availability, w/o sacrificing regional isolation for applications, and at no additional cost
    - all S3 GET, PUT, and LIST operations, as well as operations that change object tags, ACLs, or metadata, are strongly consistent. What you write is what you will read, and the results of a LIST will be an accurate reflection of what’s in the bucket.
    - 3500 PUT per sec, 5500 GET per sec
- Storage classes
    - Standard
        - no minimum storage duration requirement
    - Standard-IA
        - 30 day min storage requirement
            - will be charged for 30 days at least
        - IA, but rapid access once needed
        - for slower access, use Glacier
    - Standard 1z-IA
        - 30 days min duration
    - Glacier Instant retrieval
        - 90 days min duration
    - Glacier Flexible Retrieval
        - 90 days min duration 
    - Glacier Deep Archive
        - 180 days min duration
    - Intelligent Tiering
        - no min duration
- Lifecycle Rules
    - Transition Actions
        - Waterfall
            - S3 std 
            - S3 std-ia
            - s3 IT
            - s3 1z-ia
            - s3 glac inst retr
            - s3 glac flex retr
            - s3 glac deep arch
        - s3 to s3 1z-ia or s3-ia requires 30 days
    - Expiration Actions (auto-delete)
    - Zero-day lifecycle policy: 
        - you will want to minimize the time spent in S3 Standard for all files to avoid unintended S3 Standard storage charges. To do this, AWS recommends using a zero-day lifecycle policy. 
        - From a cost perspective, when using a zero-day lifecycle policy, you are only charged S3 Glacier Deep Archive rates. When billed, the lifecycle policy is accounted for first, and if the destination is S3 Glacier Deep Archive, you are charged S3 Glacier Deep Archive rates for the transferred files.
- Storage Class Analysis
- Requester Pays
- Event Notifications
    - Output to
        - SNS
        - SQS (only standard SQS, NOT SQS FIFO)
        - Lambda
        - EventBridge
- Performance
    - 3500 Posts, 5000 reads per sec per prefix
- Transfer Acceleration
    - Compatible with Multi-part upload
    - only pay for successful S3 TA
- ByteRange Fetches
- S3 Select
    - server side filtering for retrieving less data
    - does not support ByteRange Fetches
    - use ScanRange instead
    - max query size = 256kB
- Batch Operations
    - can be used for encrypting
    - copy modify object metadata
    - for large-scale batch operations on S3 objects, such as to copy objects, 
    - set object tags or access control lists (ACLs), 
    - initiate object restores from Amazon S3 Glacier Flexible Retrieval (formerly S3 Glacier), 
    - invoke an AWS Lambda function to perform custom actions using your objects, 
    - manage S3 Object Lock legal hold, or manage S3 Object Lock retention dates.
- Security
    - User based IAM policies
        - EC2 instance - use IAM Roles
    - Resource based
        - Bucket policies
            - for other AWS accounts
        - Object ACL
            - can be disabled
            - when another AWS account uploads an object to your S3 bucket, that account (the object writer) owns the object, has access to it, and can grant other users access to it through ACLs.
            - recommended to disable by AWS
    - Public access is disabled by default
    - Cross account S3 requests - Lambda to cross account S3 request
        - AWS Lambda can assume an IAM Role in another AWS account to access resources (e.g. S3) and do tasks (starting/stopping instances)
        - Configure your Lambda function's execution role to allow the function to assume an IAM Role in another AWS account
        - Modify your cross-account IAM Role's trust policy to allow your Lambda function to assume the role
    - S3 metadata is not encrypted
        - therefore, not recommended to put sensitive info there
    - Key Types
        - SSE-S3 (Amazon Owned and managed key)
            - each object encrypted with unique key
            - encrypts the key itself with root key, regularly rotated
        - SSE-KMS (your key in KMS)
            - may impact KMS service quota limits
        - SSE-C (bring your own Key)
        - SSE-Client (BYOK and You encrypt it)
    - enforce encryption headers with Bucket policy
    - MFA Delete
        - requires Versioning
    - Access Logs
        - stored in a separate bucket
    - Pre-signed URLs
        - timed
    - Glacier Vault Lock
        - applies WORM (write once read many)
        - uses Vault Lock Policy
    - Object Lock
        - Retention mode
            - fixed period
            - Compliance - NO ONE can delete
            - Governance - some users can delete
        - Legal Hold
            - indeifinite hold
    - Access Points
        - make it easy for groups of users to access buckets
        - has its own DNS name
        - has its own policy (simillar to bucket policy)
        - VPC origin
        - Internet Origin
    - uploader accout by default owns the object
        - to copy an object such that destination account holds it
            - add bucket policy to allow read objects on the bucket
            - create IAM policy that allows copying
            - copy using destinationa account to gain ownership
        - can create a bucket policy to require external users to grant bucket owner the object ownership
            - use `bucket-owner-full-control`
- Object Lambda
    - use to change object before retrieval
    - use cases: redacting PII, converting formats, pre-processing
- Provisioned capacity for retrievals
    - to ensure your retrieval capacity is available when you need it
    - purchase when workload requires high reliable and predictible access to a subset of data
- Server access logs
    - detailed records for the access requests made to an S3 buckets
    - vs CW
        - CW provides record of actions taken by user, rol, or AWS service
- S3 Console vs S3 Glacier Console
    - Glacier Console can be used to access the vaults and objects in them
    - however, it cannot be used to restore the object to other classes



—


# Global Infra


## CloudFront
- 216 PoPs
- DDoS protection with AWS Shield
- Origins
    - S3 Bucket
        - enhanced security with Origin Access Control
        -  can also be used as ingress
    - Custom origins
        - ALB
        - EC2 (even with ASG in front)
        - S3 static website
        - any http backend
- Origin failover
    - for HA
    - can set up primary and secondary origins
- vs S3 CRR
    - CF is good for static content
    - S3 CRR is good for dynamic content
- Geo Restriction
    - Allow list vs Block list
    - use case: copyright laws to control access
- Price class
    - All - All regions
    - 200 - exclude expensive regions
    - 100 - only least expensive (US Europe)
- Cache Invalidation
- Security
    - Field level Encryption
        - allows users to securely upload sensitive info to your web servers
        - sensitive info is encrypted at the edge
        - uses Asymmetric Keys
-  vs Global Accelerator
    -  uses multiple sets of dynamically changing IP addresses while Global Accelerator will provide you a set of static IP addresses as a fixed entry point to your applications.
    -  pricing is mainly based on data transfer out and HTTP requests while Global Accelerator charges a fixed hourly fee and an incremental charge over your standard Data Transfer rates, also called a Data Transfer-Premium fee (DT-Premium).
    -  uses Edge Locations to cache content while Global Accelerator uses Edge Locations to find an optimal pathway to the nearest regional endpoint.
    -  is designed to handle HTTP protocol meanwhile Global Accelerator is best used for both HTTP and non-HTTP protocols such as TCP and UDP.
    - Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require static IP addresses or deterministic, fast regional failover.
- Edge Functions
    - CloudFront Functions
        - lightweight
        - millions of req/sec
        - can change ViewerRequest and ViewerResponse
        - Use Cases
            - transform requests (cookie, query params, URL)
            - header manipulation
            - URL redirects or rewrites
            - Request Authn and Authr
    - Lambda@Edge
        - nodejs or python
        - 1000s of req/sec
        - can change ViewerRequest/Response and Origin Request/Response
        - write in us-east-1, CloudFront replicates functions to other locations
        - Use cases
            - File system access
            - network access
            - adjustable CPU/mem
- Access control with
    - Signed Cookies
        - allow access to content without changing URLs
    - Signed URLs
    - along with Origin Access Identifier on S3 to limit Origin to CF only



## Global Accelerator
- network layer service
- Single or Multi AWS regions
- 2 AnyCast static IPs
- works with
    - Elastic IP
    - Ec2
    - ALB, NBL (public or Private)
- Performs healthchecks
    - Great for DR
- No client cache as fixed IP
- DDOS protection thanks to AWS Shield
- DNS vs Global Accelerator for Blue/Green Deployments
    - With AWS Global Accelerator, you can shift traffic gradually or all at once between the blue and the green environment and vice-versa without being subject to DNS caching on client devices and internet resolvers, traffic dials and endpoint weights changes are effective within seconds.
    - DNS has TTL, so not suitable for B/G deployments
- use cases:
    - easily move endpoints b/w AZs or Regions w/o need for DNS config change
    - Dial traffic up or down for a region by configuring a traffic dial %
        - especially useful for testing performance and releasing updates
    - control traffic to endpoints by assigning weights




— 

# Data Transfer


## Snow Family
- If >1 week transfer over n/w, use snow
- Data Migration
    - Snowcone
        - 8-14TB
        - can use DataSync
    - Snowball Edge
        - Storage Optimized
            - 80TB HDD
        - Compute optimized
            - 42TB HDD
    - SnowMobile
        - for > 10PB
- Edge Computing
    - Snowcone
        - 2 CPUs, 4GB memory
    - Snowball Edge
        - 40 vCPUs, 80 GB RAM
- Snowball Edge offers storage clustering feature
- OpsHub
    - manage snow devices
- Snowball cannot import into Glacier directly
    - use S3 Standard with zero-day lifecycle policy



## Storage Gateway
- Bridge b/w on-prem data and cloud data
    - used for low latency access to data
        - by caching frequently accessed data on-prem
        - while storing active data in AWS
- Use internet or DX
- Use cases
    - DR
    - backup & restore
    - tiered storage
    - on-prem cache and low-latency file access
- Types
    - S3 File Gateway
        - store files in S3 using NFS, SMB
        - transitions to S3 glacier using Lifecycle policy
        - SMB has integration with AD for auth
        - Single writer, multiple readers
    - FSx File Gateway
        - enables you to store and retrieve files in Amazon FSx for Windows File Server
        - Windows native (SMB, NTFS, AD)
    - Volume
        - backed by EBS snapshots
            - can help restore on-prem volumes
        - Cached volumes
            - low latency access to most recent data
        - Stored volumes
            - entire dataset is on-prem with schedules backups to S3
    - Tape
        - cloud based VTL (Virtual Tape Library)
        - can move data directly to S3 Glacier or Glacier Deep Archive
- HIPAA eligible
- can be used with DX to increase throughput
    - traffic routed via VPC Endpoints powered by AWS PrivateLink
- can also be connected with VPN
- supported by AWS Premium Support



## DataSync
- Move/schedule sync LARGE amounts of data
    - on-prem to AWS
        - needs DataSync Agent
        - can use internet or DX
    - AWS to AWS
        - no agent needed
- does Encrypted online transfer
- data transfer faster than DX
- Can sync from/to
    - S3 (any storage class, including Glacier)
    - EFS
    - FSx (all 4)
    - other public clouds using Agent in a VM
    - NSF shares
    - SMB shares
    - HDFS (Hadoop Distributed File System)
- Data can be moved directly to Glacier
- VPC Endpoints are supported
    - increased security
    - VPC Endpoints for DataSync powered by AWS PrivateLink
- assumes IAM role when accessing S3
- can fully utilize a 10Gbps n/w
- HIPAA eligible
- vs Other services
    - DataSync - for ongoing data distribution, data pipelines, and data lake ingest, as well as for consolidating or splitting data between multiple buckets.
        - vs S3 Replication - continuous replication of data to a specific destination bucket.
        - vs S3 Batch Operation 
            - for large-scale batch operations on S3 objects, such as to copy objects, 
            - set object tags or access control lists (ACLs), 
            - initiate object restores from Amazon S3 Glacier Flexible Retrieval (formerly S3 Glacier), 
            - invoke an AWS Lambda function to perform custom actions using your objects, 
            - manage S3 Object Lock legal hold, or manage S3 Object Lock retention dates.
        - vs Snowball edge - ideal for offline data transfers, for customers who are bandwidth constrained, or transferring data from remote, disconnected, or austere environments.
        - vs Storage Gateway - once transferred with DataSync, we can use Storage File G/w to retain access to the migrated data
        - vs AWS Transfer Family - if you are currently using SFTP to exchange data with 3rd parties, provides a fully managed SFTP, FTPS, and FTP transfer directly into and out of Amazon S3, while reducing your operational burden. (DataSync provides a faster way to do this as well)



## AWS Transfer Family
- fully managed service for file transfer INTO or OUT of S3 or EFS using FTP
- Supported protocols
    - FTP
    - FTPS
    - SFTP
- Multi-AZ
- store and manager user creds within the service
- integration with existing auth systems
    - LDAP
    - Okta
    - Cognito
    - Custom
- use cases
    - sharing files
    - public datasets
    - CRM
    - ERP



—

# Decoupling

## SQS
- unlimited througput
- unlimited number of messages
- default retention: 4 days (max 14 days)
    - msg persists until a consumer deletes it
    - or max retention period is reached
- 256kb / msg
- duplicate msg possibe
- out of order msgs possible
- Amazon SQS provides common middleware constructs such as dead-letter queues and poison-pill management
- Use Cases:
    - Messaging semantics (such as message-level ack/fail) and visibility timeout.
    - Individual message delay
    - Dynamically increasing concurrency/throughput at read time
    - Using the ability of Amazon SQS to scale transparently
- consumers
    - poll sqs (can receive up to 10 msgs at a time)
- SQS Access Policies
    - useful for cross-account access
    - useful for allowing other services (SNS, S3..) to write to SQS
- message visibility timeout
    - default 30 seconds 
    - consumer should call `ChangeMessageVisibility` API to get more time
- Long Polling
    - consumers can "wait" for messages to arrive if there are none in the queue
    - decreases number of API calls to SQS
    - increases efficiency
    - wait time can be b/w 1 to 20 sec
    - preferable to short polling
    - `ReceiveMessageWaitTimeSeconds` == 0 (Default) ? Short Polling : Long Polling
- SQS FIFO
    - name of the queue must end with .ffo suffix
    - cannot convert std to FIFO
    - throughput: 300 msg / s (w/o batching), 3000 msgs/s (with batching)
        - SQS FIFO Batch Mode
            - By default, FIFO queues support up to 300 msgs/s
            - When you batch 10 msgs per operation (max value), FIFO queue can support up to 3000 msgs/sec
    - exactly-once send capability (by removing duplicates)
    - msgs processed in order
    - deduplicates
        - possible to introduce duplicates by producer
            - but removed within 5 minutes
    - msgs can be bundled with a group ID
        - all messages within a group are in strict order
    - not allowed as destination for S3 Event Notifications
        - only Standard is allowed
- SQS can act as a buffer to DB writes (Aurora, RDS, DynamoDB)
- vs Kinesis Data Streams
    - real time processing of streaming big data
    - ability to read and replay records to multiple Amazon Kinesis Apps
- Security
    - can use SSE for sensitive data
- Dead letter Queues: 
    - is an Amazon SQS queue to which a source queue can send messages if the source queue’s consumer application is unable to consume the messages successfully.
- SQS temporary queues
    - helps save dev time and cost
    - use common message patterns such as request-reponse
    - high throughput



## SWF
- Simple Workflow Service
- apps that coordinate work across distributed components



## SNS
- push based delivery (no pull)
- 12.5M subscriptions per topic
- 100k topic limit
- inputs
    - CW Alarms
    - AWS Budgets
    - Lambda
    - ASG (notifications)
    - S3 bucket (events)
    - DynamoDB
    - CloudFormation (state changes)
    - AWS DMS (for new replica)
    - RDS Events
- Security
    - Access control with IAM policies
    - OR with SNS Access Policies (simiar to Bucket policies)
        - useful for cross-account access to SNS
        - useful for allowing other services to write to an SNS topic
- useful when we want to send one S3 event to many SQS queue using fan-out construct
- can send data to Kinesis Data Firehose
- SNS FIFO
    - Ordering
    - Deduplication
    - Limited throughout (same as SQS FIFO)
- Message Filtering
    - JSON policy used to filter msgs
    - if a subscription does not have a filter policy, it receives all msgs
- HIPAA eligible
- Stores msgs in multi-AZ disks
- msgs cannot be recalled once published
- vs SES
    - use SNS with CW
    - use SNS when monitoring related emails are to be send
    - use SES for transactional emails


## Kinesis Data Streams (KDS)
- Streaming service for ingest/process at scale
    - KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources 
    - sources: 
        - website clickstreams
        - database event streams
        - financial transactions
        - social media feeds
        - IT logs
        - location-tracking events
    - process means
        - transforming before emitting to data store
        - running real-time metrics and analytics
        - deriving more complex data streams for further processing
- Real time (~200 ms)
    - combine with following for real-time data analysis
        - Kinesis Data Analytics
        - Lambda
        - EC2
        - Spark on EMR
- Producers
    - Custom apps and clients
    - SDK, KPL (kinesis producer Library)
    - Kinesis Agent
- Consumers
    - Apps (KCL, SDK)
    - Lambda
    - Kinesis Data Firehose
    - Kinesis Data Analytics
- Record
    - Input record
        - Parition key
        - data blob up to 1MB
    - Output record
        - Partition Key
        - Sequence no.
        - Data Blob
- throughput
    - Input
        - 1 MB/sec or 1000 msg /sec per shard
    - Output
        - shared
            - 2MB/sec (shared) per shard per all consumers
        - enhanced fan-out feature
            - 2MB/sec per shard per consumer
    - To increase throughput:
        - increase number of shards
            - costly
        - send data in batches
            - cost effective
- Capacity modes
    - Provisioned
        - 1MB/sec IN, 2MB/sec out
        - You pay per shard provisioned / hr
    - On-Demand mode
        - default capacity provisioned (4MB/s or 4k records/sec)
        - pay per stream per hour & data in/out per GB
    - can switch modes twice a day
- Retention
    - 1 day - 365 days
    - ability to replay / reprocess
    - data is immutable
- Security
    - use IAM policies
    - VPC Endpoints for kinesis available for access within VPC
    - can use SSE to encrypt data
- vs Other services
    - Kinesis DS recommended for
        - Routing related records to the same record processor 
        - ordering of records
        - Ability for multiple applications to consume the same stream concurrently.
        - Ability to consume records in the same order a few hours later.
    - SQS
        - Messaging semantics (such as message-level ack/fail) and visibility timeout.
        - Individual message delay
        - Dynamically increasing concurrency/throughput at read time
        - Using the ability of Amazon SQS to scale transparently
- Kinesis Video Streams
    - securely stream videos from connected devices to AWS



## Kinesis Data Firehose (KDF)
- Load streaming data / Streaming ETL Solution
- Auto scaling
- no data storage
- no replay capability
- Input data can be ordered using "Partition keys" per shard
- Near real-time
    - 60 seconds latency min for non-full batches
    - or min 1MB of data at a time
- Inputs
    - Apps and clients
    - SDK, KCL
    - Kinesis Agent
    - Kinesis Data Streams
    - Amazon CloudWatch (Logs and events)
    - AWS IoT
- can dump data in a single data repository at a time
- Outputs
    - 3rd party
        - Splunk
        - New Relic
        - DataDog
        - Mongodb
    - AWS
        - S3
        - Amazon Redshift (through S3 COPY)
        - ElasticSearch
    - custom endpoints
- Failed data goes to S3 Backup bucket
- Can invoke a Lambda for data transformation
- Record
    - up to 1 MB


## Active MQ
- managed broker service for RabbitMQ, ActiveMQ
- does not scale as much as SQS/SNS
- Highly available
    - can run in Multi-AZ with failover
- has both queue feature (~SQS) and topic feature (~SNS)




—


# Containers

## ECS
- Launch Types
    - Ec2
        - must provision and maintain the infra (the ec2 instances)
        - instances must run ECS Agent
            - uses EC2 Instance profile
    - Fargate
        - Serverless
        - Fargate + EFS = Serverless
- EC2 Task Role - for each task
- Tasks can be Target Groups for ALB/NLB 
- Auto-Scaling
    - task level
    - uses AWS Application Auto Scaling
    - Target Tracking
    - Step Scaling
    - Scheduled Scaling
- Tasks can be invoked by EventBridge
- Networking modes in Amazon ECS tasks Ec2 launch type
    - Host Mode
        - n/w of container tied to underlying host
            - basic
    - Bridge mode
        - port remapping b/w host & container ports
    - None mode
        - no external connectivity for containers
    - AWSVPC mode
        - seperate ENI, IP and SG for each task
        - helps for separate security policies for each task
        - and get granular traffic monitoring
- ALB supported and works 
    - ECS Tasks can be target groups for ALB
- NLB only for high thoughput / performance
    - or pair it with AWS PrivateLink
- ELB not supported



## EKS
- Types of Node Groups
    - Managed Node group
        - creates and manages nodes for you
        - Supports on-demand or spot instances
    - Self-Managed Nodes
        - nodes created by you managed by ASG
        - can use prebuilt AMI (Amazon EKS Optimized AMI)
        - Supports on-demand or spot instances
    - AWS Fargate
        - no maintenance required
        - No nodes managed
- Data volumes
    - can use EBS, EFS, FSx
- EKS Anywhere
    - can be used for deploying k8s anywhere
    - uses VMWare vSphere


# Fargate
- managed container service
- Fargate tasks are given 20GiB of free ephemeral storage



—


# Serverless



## Lambda
- CRON job with CloudWatch Events or EventBridge
- Limits per region
    - memory 128MB to 10GB (1MB increment)
    - maximum execution time = 15 minutes
    - Env vars = 4kb
    - disk capacity = 512MB to 10GB
    - Concurrency executions: 1000 (can be increased)
    - Deployment size = 50MB (compressed), 250MB uncompressed
    - can use `/tmp` directory to load other files
- Lambda in VPC
    - Lambda functions always operate from an AWS-owned VPC with full internet access by default
    - in VPC, creates an ENI in your subnets
    - Once your function is VPC-enabled, all network traffic from your function is subject to the routing rules of your VPC/Subnet. If your function needs to interact with a public resource, you will need a route through a NAT gateway in a public subnet.
    - You should only enable your functions for VPC access when you need to interact with a private resource located in a private subnet. An RDS instance is a good example.
- Can be invoked from RDS/Aurora
    - process data events
    - DB Instance must have the required permissions (Lambda Resource-based Policy and IAM Policy)
- Lambda Layers
    - provide a convenient way to package libraries and other deps
    - using layers reducees the size of uploaded deployment archives
        - makes deployment faster
    - allows code reuse
    - lambda can be configured to pull in additional code in the form of layers
    - can use up to 5 layers at a time
    - Layers support resource-based policies
- Env Variables
    - Lambda encrypts Env Vars by default using the default key
    - however, that data is still visible/accessbile on Lambda Console
    - for even greater security, create your own KMS key and use it to encrypt Env Vars

—



## DynamoDB
- Std and IA Tables
- item size max = 400KB
- Read/Write capacity modes
    - Provisioned
        - plan RCU/WCU beforehand
        - possible to add auto-scaling mode
    - On-Demand
        - no capacity planning needed
- DAX
    - helps resolve congestion by caching
    - 5 minutes TTL for cache
- DynamoDB Streams
    - Ordered stream of item-level modifications (CRUD)
    - invoke AWS Lambda
    - 24 hour retention
- Global Tables
    - make a DynamoDB table accessible in multiple regions with low latency
    - Must enable DynamoDB Streams
- Item TTL
    - auto delete item after expiry
- DR and Backups
    - continuous backups & PITR
        - optionally enabled for last 35 days
    - On-Demand Backups
        - does not affect performance
- Integration with S3
    - exports to S3 (must enable PITR)
    - import from S3
        - csv, JSON or ION
- Partition Keys
    - use P Keys with high-cardinality values to use provisioned throughput efficiently



## API Gateway
- can cache API responses
- Integrations
    - Lambda
    - HTTP
    - AWS Service
    - ACM
        - custom domain name HTTPS
        - for edge-optimized must be in us-east-1
        - must use CNAME or A-alias record
- Endpoint Types
    - Edge-Optimized
        - requests routed through CloudFront edge locations
        - the g/w still lives in 1 region
    - Regional
        - for clients within same region
    - Private
        - can only be accessed using VPC Endpoints
        - use a Resource-based policy to define access
- Can use Mapping templates 
    - to map the payload from a method request to integration request
    - written in Velocity Template language
- throttling & quotas
    - to help protect APIs from being overwhelmed by spikes
    - throttling at multiple levels including 
        - global 
        - by a service call
    - any request over the limit receives a 429
    - uses token bucket algorithm



## Cognito
- User Pools
    - Sign in functionality for app users
    - Integrates with
        - API G/w
        - ALB (NOT cloudfront)
- Identity Pools (Federated Identity)
    - provides temp creds for accessing AWS resources
    - integrates with Cognito User pools as IdP





# Data Analytics

## Athena
- Analyze data in S3 using Serverless SQL
- use standard SQL
- supports
    - csv
    - json
    - parquet
    - avro
    - ORC
- commonly used with QuickSight for dashboards/reporting
- Use Cases
    - Business Intelligence
    - analytics
    - analyze and query
        - VPC logs
        - ELB Logs
        - CloudTrail trails
- Use columnar data




## Redshift
- used for OLAP
- 10x better performance than other data w/h
- columnar storage
- vs Athena
    - faster queries/joins/aggregations due to indexes
- Redshift Cluster
    - Leader node + compute nodes
    - can use reserved instances
- Redshift has multi-AZ for some clusters
- Backups
    - snapshots stored in S3
    - snapshots are incremental
    - can configure to copy snapshots to another region
- Loading data
    - Kinesis Data Firehose
    - S3
        - internet
        - or Enhanced VPC Routing
    - JDBC driver on Ec2
- Redshfit Spectrum
    - query data already in S3
    - must have Redshift cluster to start the query
- Cross Account Data Sharing
    - use datashare
        - a unit of sharing data that can be shared in same or different accounts
- AQUA (advanced query accelerator)
    - boost query perf by 10x


## OpenSearch
- search any field, including partial matches
- does NOT support SQL
- Ingestion from
    - Kinesis DF
    - AWS IoT
    - CloudWatch Logs


## Glue
- managed ETL service
- Serverless
- useful to prepare and transform data for analytics
- can convert data into different formats
- Glue Data Catalog
    - catalog of datasets
    - Glue Data Crawler writes metadat to Glue Catalog
    - Data can then go to
        - Athena
        - Redshift Spectrum
        - EMR
- Glue Job Bookmarks
    - prevent re-processing old data


## EMR
- industry leading big-data platform
- PB scale
- using tools like
    - Apache Spark
    - Apache Hive
    - Apache HBase
    - Apache Flink
    - Presto
- used for launching Hadoop/Spark clusters


## Lake Formation
- Data Lake
- central place to have all your data for analytics
- combine structured and unstructured data
- Fine grained access control for data (row and column level)
    - Fine grained centralized permissions
- Built on top of Glue



## Kinesis Data Analytics
- For SQL apps
- Real time on KDS and KDF using SQL
- Serverless
- Use cases
    - time-series analysis
    - Real-time dashboards
    - Real-time metrics




—

# ML

## Rekognition
- find objects, people, text, scenes in images and videos using ML
- facial analysis and facial search for user verification
- use cases
    - content moderation in media
    - celebrity recognition


## Transcribe
- convert speech to text
- use Automatic Speech Recognition
- Auto remove PII
- support Auto Lang detection
- supports speaker identification
    - label individual speakers
    - identify 2-10 speakers in an audio clip


## Polly
- text into speech
- Pronunciation lexicons
    - customize pronunciation
- SSML (synthesis markup lang)
    - emphasize words or phrases
    - including breathing sounds etc
- Brand Voice
    - create custom NTTS (Neutral Text to Speech) voice
    - used for adverts
    - used to create unique vocal identifiers for specific products

## Lex
- speech to text using NLP
- helps builds conversational chatbots, call center bots
- used to build Alexa
    - Alexa cannot be used as Auto Speech Recognition service
        - althought internally it uses ASR

## Comprehend
- NLP
- use ML to uncover insights and relationships in your unstructured data
- the service
    - identifies the language of the text
    - extracts key phrases
    - places
    - people
    - brands 
    - events
- can integration with Lambda and ElasticSearch (& its dashboard for visualization)

## Comprehend Medical
- NLP service
- use ML to extract relevant medical info from unstructured text
- can quicky gather accurate info like
    - medical conditions
    - medication
    - dosage
    - strength
    - frequency
- HIPAA eligible
- can quickly identify Protected Health Info (PHI)


## Textract
- extract text from documents like pdf

## SageMaker
- devs/data scientist can build ML models



—



# Monitoring



## CloudWatch
- Available Metrics
    - CPUUtilization
    - network
- Metrics belong to namespaces
- metrics have timestamps
- can create CW Custom Metrics 
    - RAM/Mem usage
    - Disk space utilization
    - Disk swap utilization
    - Page file utilization
    - Log collection
- CW Metric Streams
    - near-real time delivery
        - KDF
        - 3rd party (DataDog, Splunk)
    - use filter metrics to stream a subset
- CW Logs
    - outputs
        - S3 (export)
            - can take 12 hours
        - KDS
        - KDF
        - Lambda
        - OpenSearch
    - sources
        - SDK
        - CW Logs Unified Agent
            - metrics
                - CPU
                - Disk metrics
                - RAM
                - netstat
                - processes
                - swap space
        - Elastic BS
        - ECS
        - Lambda
        - VPC Flow logs
        - API G/w
        - CloudTrail based on filter
        - DNS Logs
    - can use filter expressions
        - e.g. ERROR occurences
        - can be used to trigger CW alarms
    - CW Logs Subscriptions
        - real-time
- CW Alarms
    - states
        - OK
        - INSUFFICIENT_DATA
        - ALARM
    - targets
        - EC2
        - ASG
        - SNS
    - can create composite alarms
- Container Insights
    - metrics and logs from containers
- CW Lambda Insights
- CW Contributor insights
    - top-N contributors (e.g. top 10 IPs)
    - leverages CW Logs
- CW Application Insights
    - powered by SageMaker
    - automated dashboards
    - for Java/.Net 



## EventBridge
- Cron jobs
- Events
- Sources
    - EC2
    - CodeBuild
    - S3
    - Trusted Advisor
    - CloudTrail
    - cron
- Destinations
    - Compute
        - Lambda
        - AWS Batch
        - ECS Tasks
    - Integration
        - SQS
        - SNS
        - KDS
    - Orchestration
        - Step functions
        - CodePipeline
        - CodeBuild
    - Maintenance
        - SSM
        - EC2 actions
- can archive and replay events
- use Resource based policy 
    - to allow/deny events from other AWS accounts or regions
    - can aggregate all events from your Organization in single AWS account
- vs SQS
    - SQS does not integrate with 3rd party
- Event Buses
    - default Event Bus
        - connect to AWS services
    - Partner Event Bus
        - connect to 3rd party like zendesk and Datadog
    - Custom Event Bus
        - connect to custom apps


## CloudTrail
- records API acitivity in AWS account
- governance, compliance and audit for AWS Account
- get history of API calls
- a Trail can be All region (default) or multi-region
- Inputs
    - SDK
    - CLI
    - Console
    - IAM Users and Roles
- Outputs
    - CW Logs
    - S3 Bucket
- CloudTrail Events
    - Mgmgt Events
        - operations on resources in AWS (e.g. attach policy)
        - logged by default
    - Data Events
        - not logged by default
    - Insight events
        - detect unusual activity
- Retention
    - 90 days
    - for longer, log to S3 and use Athena
- CloudTrail Log File integrity validation feature
    - to check if a log file was modified or deleted or unchanged
- Encryption
    - SSE enabled by default


## Config
- auditing and recording compliance
    - record config changes
    - evaluate resources againt compliance rules
- can receive SNS alerts
- per region service
    - but can be aggregated across regions and accounts
- Config Rules
    - Remediations
        - trigger auto remediation action
    - Notifications
        - use EventBridge/SNS to trigger notifications





—





# IAM
- IAM Conditions
    - aws:SourceIP
    - aws:RequestedRegion
    - ec2:ResourceTag
    - aws:MultiFactorAuthPresent
- IAM Role
    - when you assume a role (user, app or service), you give up your original permissions and take permissions assigned to the role
    - Does not happen if using Resource-based policies
- Permission Boundaries
    - supported for users and roles (NOT groups)
    - Use cases
        - delegate responsibility to non-admins within their perm bound
        - allow devs to self-assign policies within limits
        - useful to restrict one specific user


## Organizations
- Global Service
- Shared reserved instances
- Savings plan discounts
- SCP (Service Control Policies)
    - IAM policies applied to OUs
    - do not apply to management account
    - must have an explicit allow

## IAM Identity Center
- SSO
- SAML2.0 web apps
- Manage access across AWS Accounts
- ABAC 
    - fine grained permissions based on users attributes stored in IAM-IC Identity store



## Directory Services
- AWS Managed Microsoft AD
    - run directory-aware workloads in the cloud
    - configure trust relationship b/w
        - AWS Managed AD
        - existing on-prem Microsoft AD
- AD Connector
    - proxy
    - allow on-prem users to log in to AWS apps with their AD creds
- Simple AD


## Control Tower
- set up and govern a secure and compliant multi-account AWS env
- automate the set up of your envs
- automate ongoing policy mgmgt using GuardRails
    - Preventive GuardRail
        - using SCPs (e.g. restrict regions in all your accounts)
    - Detective GuardRails
        - using AWS Config (identify untagged resources)





—


# Security



## KMS
- Types of Keys
    - AWS Owned (SSE-S3, SSE-SQS, SSE-DDB)
        - free
        - default
    - CMK
    - Customer managed imported
- Auto key rotation
    - AWS Managed
        - 1 year auto
    - CMD
        - 1 year must be enabled
    - imported
        - only manual
- Creating snapshots across accounts
    - create snapshot
    - encrypt with your key (CMK)
    - attach a KMS Key Policy to authorize cross-account access
    - Share the encrypted snapshot
    - create copy of snapshot and encrypt with your CMK
    - create a volume from snapshot
- Multi-Region Keys
    - not global, so primary + replica
    - use cases
        - global Client side encryption
        - encryption on Global DynamoDB and Global Aurora
    - treated as independent keys by S3
        - so objects will be decrypted, replicated and then encrypted


## SSM Parameter store
- secure storage for configs and secrets
- Serverless
- version tracking
- notifications with EventBridge
- allows a TTL assignment to force update or delete
- vs Secrets Manager
    - SSP Parameter store is cheaper
    - recommended for app parameters like db passwords etc


## Secrets Manager
- newer than SSM
- force rotation of secrets every X days
- integration with
    - RDS
    - encrypted using KMS
- replicates secrets across regions
    - use cases
        - multi-region apps
        - DR
        - multi-region DB
- vs SSM Parameter store
    - SM allows auto rotation
    - is mainly used for keys, not app params
    - SM is more costly


## ACM
- integration with
    - ELB
    - CloudFront Distributions
    - API G/w
- for public certs
    - ACM sends daily expiration events starting 45 days prior to expiry
- ACM issued certs valid for 395 days
    - Email validated certs require action by domain owner
    - email contains a link for easy renewal
- ACM lets you import 3rd party certs
    - if ACM is not available, IAM cert store can be used


## WAF
- Layer 7
- Deploy on
    - ALB
    - API G/w
    - CloudFront
    - Cognito User Pool
    - NOT on EC2 instances directly
- Define Web ACL
    - can filter out IPs
    - can block countries
    - rate-based rules for DDOS protect
- rate-based rules:
    1. Blanket rate-based rules
        - prevents source IP address from making excessive requests to entire app
    2. URI specific rate-based rule
        - prevents IP address from making excessive requests to a particular URI
        - These are useful if specific functions of the app, e.g. computationally expensive resources need to be protected
    3. IP reputation rate-based rule
        - Prevents well-known malicious IP addresses from making excessive requests to the application
    - All the above rules can be combined or can be used separately as per need 


## Shield
- protect against DDoS


## Firewall Manager
- Manage rules in all accounts of Organizations
- As new applications are created, Firewall Manager makes it easy to bring new applications and resources into compliance by enforcing a common set of security rules
- resource it can configure across accounts
    - AWS WAF rules
    - AWS Shield Advanced protection
    - VPC SGs
    - AWS Network Firewalls
    - Route53 DNS Resolver Firewall
- DOES NOT support NACLs

## Guard Duty
- threat discovery
- input data
    - CT Log events
    - VPC Flow Logs
    - DNS Logs
    - EKS Logs
- can set up EventBridge rules for notifications


## Inspector
- security assessments
- find CVEs


## Macie
- ML and pattern matching to discover sensitive data and PII in AWS


## Artifact
- central resource for all compliance-related info


—


# VPC
- 5 vpcs per region (can be increased)
- vpc can span multiple AZs
    - however, subnets are in 1 AZ
    - if instances are in different subnets which are in different AZ
        - $0.01 per GB data transfer
- 5 max CIDRs per vpc
    - mix size /28
    - max size /16
- only private IP ranges allowed
- Subnet
    - first 4 and last IP reserved by AWS
- NACL
    - operate on subnet level
    - stateless
        - hence both inbound and outbound rules needed
    - Rules have number (1-32766)
    - lower number = higher precendence
    - Use case
        - block specific IP/CIDR at subnet level
- VPC Peering
    - Privately connect 2 VPCs using AWS n/w
    - must not have overlapping CIDRs
    - not transitive
        - i.e. must establish b/w each pair of VPCs
        - must update route table in each subnet
    - can connect VPCs in different regions
    - can reference SG of peered VPC within same region
    - can connect to VPC in another account
        - need them to accept
- VPC Endpoints
    - allows connection to AWS services using private network
    - remove the need for IGW, NATGW
    - Types
        - Interface
            - powered by PrivateLink
            - provisions an ENI (must attach an SG)
            - supports most AWS services
            - $ per hour + $ per GB
            - Preferred over G/w only when:
                - access is required from on-prem (Site-to-Site VPN or DX)
                - a different VPC
                - different region
        - Gateway
            - must be used as a target in Route table
            - S3 and DynamoDB
            - Free
            - usually preferred over Interface
- VPC Flow Logs
    - capture info abt IP traffic to your interfaces
        - VPC + subnets + ENI
        - also ELB, RDS, ElastiCache, Redshift, Workspaces, NATGW, Transit G/w
    - can output to
        - CW Logs
            - for CW Contributor insights or Alarms
        - S3 Bucket 
            - for analysis by Athena
- Traffic Mirroring
    - capture and inspect n/w traffic in VPC
    - simply copies the traffic, no analysis
        - for analysis, use N/w Firewall
- VPC Wizard options
    - single public subnet
    - public and pvt subnets
    - public and pvt subnets + Site-to-Site VPN Access
    - pvt subnets only + Site-to-Site VPN access
- For instances without public IPs to access internet
    1. use NATGW and its public IP 
    2. with h/w VPN or DX, route the traffic through VPGW to existing DC
- DNS resolution and DNS hostname config
    - instance launched in default VPC
        - provided with public and private DNS hostnames
            - correspond to public and private IPv4 add of the instacnes
        - instance launched in non-default VPC
            - provided with a private DNS hostname only
            - public hostname when DNS resolution and DNS hostname config are enabled




## IGW
- Allows VPC to connect to internet
- two purposes
    - to provide a target in your VPC route tables for internet-routable traffic
    - perform NAT for instances that have public IPv4 addresses
- scales horizontally
- no bandwidth constraints
- HA
- 1 VPC ↔ 1 IGW
- for internet access Route tables must be edited
- Egress-only IGW
    - for IPv6


## NAT Gateway
- allows private instances to connect to IGW
- HA
- limited to specific AZ
- uses an Elastic IP
- does not support (unlike NAT instances) 
    - SGs
    - Bastion Host
    - Port forwarding
- connectivity types
    - Public 
        - for instances in private subnets to connect to IGW
        - connect VPCs or on-prem n/ws using Transit gateway OR VPGW
    - Private
        - cannot connect to IGW as IGW drops traffic from pvt NATGW
        - connect other VPCs & on-prem 


## Site-to-Site VPN
- connect on-prem n/w to VPC
- IGW not required
- connection sides
    - Virtual Private Gateway (VPGW) on AWS side
    - Customer Gateway (CGW)
        - s/w or physical device
- CGW (on-prem)
    - use public internet-routable IP addr
- Enable Route Propagation for VPGW
- enable ICMP protocol for pinging EC2 instances
- Max throughput of 1.25Gbps
    - to scale, use ECMP (Equal cost multi-path) enabled transit gateway over multiple VPNs

## VPN CloudHub
- secure connections between multiple VPNs and different sites
- low cost hub and spoke model
- VPN connection, so goes over public internet
- uses VPGW with multiple CGW


## Direct Connect (DX)
- dedicated private connection to VPC
    - connection must be set up b/w your DataCenter and AWS DC locations
- must set up a VPGW on your VPC
- can access public (e.g. S3) and private (EC2) resources on same connection
    - by using public and private Virtual Interfaces (VIFs)
- Use cases
    - increases b/w throughput
    - more consistent n/w experience
    - Hybrid environments (on-prem + cloud)
- supports both IPv4 and IPv6
- Connection Types
    - Hosted
        - 50Mbps 500Mbps to 10Gbps
        - capacity can be added or removed on demand
    - Dedicated
        - 1gpbs, 10 gbps, 100 gbps
- lead times are often longer than a month
- Security
    - Data is not encrypted but private
    - use DX + VPN for IPSec-encrypted private connection
- Resiliency
    - use separate connections for better
- Can use Site-to-Site VPN connection as a backup in case DX fails


## Direct Connect Gateway
- for DX to one or more VPC in many different regions
    - Instead of connecting the on-prem data center to multiple VPCs individually over the DX link, you can deploy the Direct Connect Gateway.
- grouping if VPGWs and private VIFs
- globally available resource
    - create in one region, access from any other region


## Transit Gateway
- transitive peering connections b/w thousands of VPC and on-prem
- hub and spoke star connection
- attach all your hybrid connectivity (VPN & DX) to it
    - consolidating and controlling your org's entire AWS routing config in 1 place
- regional resource
    - but can work cross region
    - can peer Transit Gateways across regions
- share cross account using Resource Access manager 
- works with Direct Connect Gateway, VPN Connections
- supports IP Multicast
- create multiple Site-to-Site VPN connections to increase your bandwidth
- allows sharing DX b/w multiple accounts

## PrivateLink
- tech behind VPC Endpoints
- provides secure connectivity for services behind separate VPCs
    - used in conj. with NLB


## Network Firewall
- protect entire AWS VPC
- filter traffic at the perimeter of the VPC
    - to and from 
        - IGW
        - NATGW
        - VPN
        - DX
- from Layer 3 to Layer 7
- Traffic Filtering
    - allow, drop or alert for traffic that matches the rules
    - can create allow lists and block lists with FQDNs
- send logs of rule matches to
    - S3
    - CW Logs
    - KDF
- use cases
    - pass traffic only thru known domains or IP address endpoints
    - use custom lists of known bad domains
    - perform deep packet inspection



—



# MISC



## DMS
- supports homogenous and hetro migrations
- must create Ec2 instance running DMS for migration and replication
- uses Schema conversion tool for 1 db engine to another
- Continuous Data Replication
    - on-prem to AWS or vice-versa
- can encrypt source and target endpoints by using SSL certs
- can be used to migrate RDS Aurora to 
    - Aurora Serverless
    - Aurora in different region


## Backup
- supports cross region
- supports cross accounts
- on-demand and scheduled backups
- backup plans
    - backup frequency
    - backup window
    - transition to cold storage
    - retention period
- Vault Lock
    - enforce WORM
    - even root users cannot delete backups


## Application Discovery
- plan on-prem to AWS migrations
- modes
    - Agentless
        - for VMWare
    - Agent-based
        - system config
        - system performance
        - running processes
        - details of n/w connections


## Application Migration Service
- to perform the actual migration
- beings by installing the AWS Replication Agent on your source servers



## RAM
- securely share resources across AWS accounts within Org or OU
- can also share with IAM roles and users for supported resources
- eliminates the need to provision and manage resources in every account
- can share
    - Transit g/w
    - subnets
    - License Manager configs
    - Route53 Resolver rules


## CloudFormation
- each resource within stack is tagged with an id so you can see how much a stack costs
- can estimate costs using CloudFront Template
- Custom Resources
    - can be used to write custom provisioning login
    - Either associated with 
        - SNS backed customer resources
        - Lambda backed
            - Lambda is invoked when a custom resource is CRUD
- template anatomy
    - Version
    - metadata
    - parameters
        - values to pass at runtime
    - rules
        - validate parameters
    - mappings
        - KV pairs for conditional parameter values
    - conditions
    - transform
    - resources
    - outputs
        - values that can be imported to other stacks to create cross-stack references
- use `CreationPolicy` attribute ona resource to prevent its status from reaching completion
    - unless CloudFormation receives a specified number of success signals
        - OR
    - the timeout is exceeded

## X-Ray
- helps you analyze and debug modern micro-services apps and serverless archi
- quantify customer impact
- collect data about requests that your app serves

## Outposts
- delivers AWS infra to any on-prem or edge location
- provide fully managed solution
- its kinda opposite of Storage G/w, 
    - so used when AWS services want to access on-prem data
        - that cannot be transferred to AWS


## Systems Manager
- SSM Session Manager
    - start a secure shell on ec2 and on-prem
    - even when ssh on port 22 is disabled
- SSM Run Command
    - execute a document or script across multiple instances
    - no need for SSH
- Patch Manager
    - automatic patch mgmgt for instances
    - patch on-demand using maintenance windows
- Automation
    - simplify common maintenance tasks
        - e.g. restart instances, create an AMI, EBS snapshot



## Well-Architected Framework
- helps cloud architects build secure, high performing, resilient and efficients apps
- battle tested workloads reviews by other architects
- provides a consistent approach for evaluating your cloud arch
    - and implementing designs that will scale your app need over time


## AWS Health
- provides ongoing visibility into your resource perf & availability of your AWS services
- can use AWS health events to learn how service and resource changes affect your app


## Trusted Advisor
- inspects your AWS env and makes recommendations
    - to save money
    - improve system avail. and perf.
    - or close security gaps
- can run service limit checks
- checks in the following categories
    - Cost Optimization
    - Security
    - Fault Tolerance
    - Performance
    - Service Limits


## AppSync
- serverless GraphQL and Pub/Sub API Service