# AWS compute services

1. Amazon EC2
2. Amazon Lambda
3. AWS Batch
    - run Batch computing workloads
4. AWS Elastic Beanstalk
    - Automates deployment, mgmt, scaling and monitoring of you custom apps in AWS
5. AWS LigthSail
    - easy to use Virtual private Servers
6. AWS Outposts
    - Hybrid service that allows us to AWS services in our on-prem data center

AWS Batch and AWS EBS are Orhestration services


# AWS Container Services

- Amazon ECS
- Amazon EKS
- AWS Fargate
    - AWS Fargate is a serverless, pay-as-you-go compute engine for containers that lets you focus on building applications without managing servers.
- Amazon ECR
    - Fully managed docker container registry
- AWS App2Container or A2C
    - transforms .Net or Java apps to containerized apps


# AWS Database Services

- Amazon RDS
- Amazon Aurora
    - Both above capable for OLTP
- Amazon Redshift
    - Used as a data warehouse (OLAP)
- Amazon DynamoDB
- Amazon DocumentDB
    - compatible with mongodb
- Amazon Elasticache - redis or memcached
- Amazon Keyspaces
    - Apache Cassandra compatible
        - Cassandra is wide column data store that is designed to handle large amounts of data
- Amazon Nepture
    - fully-managed Graph DB service
- Amazon Timestream
    - Serverless time series DB for IoT and operational applications
- Amazon Quantum Ledger (QLDB)
    - fully-managed ledger DB
    - a transparent and immutable transaction logs (e.g. audit logs)


# Application Integration Services

- Amazon SQS (Simple Queue Service)
- Amazon SNS (Simple Notif. Service)
- AWS Step Functions
    - orchestrator for AWS lambda - coordinate multiple Lambda functions
- Amazon MQ
    - Uses the open source Apache ActiveMQ
- Amazon EventBridge
- AWS AppSync
    - A managed service that uses GraphQL
- Amazon AppFlow
    - fully managed integration service that enables secure transfer of data b/w various systems such as your SaaS apps and AWS Services


# Analytics Services

- Amazon Kinesis
    - Analyze data streams in real-time
- Amazon Athena
    - interactive query service for data stored in S3, using standard SQL
- Amazon ES (Elasticsearch)
- Amazon EMR (Elastic MapReduce)
    - Not Serverless
- Amazon QuickSight
    - Create embeddable dashboards
- Amazon CloudSearch
- Amazon Redshift
    - Not Serverless
    - fast scalabale data warehouse
- AWS Data Pipeline
    - process and move your data b/w different AWS compute and storage services
- AWS Glue
    - ETL workloads (Extract, transform and load)
- Amazon Managed Streaming for Apache Kafka
- AWS Lake Formation


# Identity Services

- AWS IAM (Identity and Access Mgmt)
- AWS Single Sign-on
- AWS Cognito
    - add user-sign-up signin and access control features to web or mobile app
- AWS Directory Service
    - A managed MS Active Directory


# Storage Services

- Amazon EBS (Elastic Block Store)
    - persistent block-level storage
- Amazon S3
- Amazon S3 Glacier
    - used for cold data - data accessed less frequently
- Amazon EFS (Elastic File System)
    - POSIX compliant shared File Storage system
- Amazon FSx for Lustre
- Amazon FSx for Windows File Server
- AWS Backup
- AWS Storage Gateway
    - connects on-prem storages to AWS storage services


# Monitoring Services

- Amazon CloudWatch
    - metrics repository for both aws and on-prem services
- AWS Service Health Dashboards
    - shows AWS Service availability
- AWS Personal Health Dashboard
    - status of AWS services in use by us
- AWS Health API
    - programmatic access to AWS Personal Health Dashboard


# Audit Services

- AWS CloudTrail
    - Tracks user activity and API usage
- AWS Artifact
    - Provides on-demand AWS security and compliance reports
- AWS Security Hub
    - Provides a centralized & comprehensive view of the security postureof your cloud infra across multiple AWS accounts


# Networking and Content Delivery Services

- Amazon VPC
    - Logical private network within the cloud
- Elastic load Balancing
    - Auto distributes incoming traffic across multiple targets
- Amazon Route 53
    - a DNS web service
- AWS Global Accelerator
    - Provides a set of static anycast IP addresses
- Amazon CloudFront
    - a CDN service
- AWS PrivateLink
    - allows private connectivity to various AWS services, without passing through the public internet
    - provides a private endpoint aka VPC Endpoint
- AWS VPN
    - enables secure connection of your on-prem networks to AWS
- AWS Direct Connect
    - allows to establish a dedicated network connection from your on-prem network to AWS
    - the data does not pass through public internet
- AWS Transit Gateway
    - connects amazon VPCs, VPNs, Direct Connect Gateways and on-prem n/w to a single gateway
    - recommended for large organizations
- Amazon API Gateway
    - allows us to publis, maintain, monitor and secure RESTful APIs
- AWS App Mesh
    - A service mesh (in infra layer that handles comms b/w microservices) that provides app-level n/w for different types of containerized apps in AWS
- AWS Cloud Map
    - cloud resource discovery service
    - used in microservices and containerized apps that have dynamically changing resources


# Security Services

- AWS WAF (Web Application Firewall)
    - protects web apps from common web exploits
- AWS Firewall manager
    - a security management service designed for AWS WAF rules
    - allows to centrally configure and manage WAF rules across multiple AWS accounts
- AWS Shield
    - a managed DDoS protection service
- Amazon GuardDuty
    - a managed threat detection service that identifies malicious or unauthorized activities in your AWS accounts
- AWS CloudHSM (H/w security module)
    - a managed cloud-based HSM (allows to easily generate and use our own encryption keys)
- AWS Key Management Service (AWS KMS)
    - internally uses HSMs for creating and controlling encryption keys, but has multi-tenant access
- AWS Secrets Manager
        - protect the secret of your apps, services and IT resources
- AWS ACM (Certificate Manager)
    - manage and deploy SSL/TLS certs
- Amazon Macie
    - a fully managed data security and data privacy service that auto recognizes and classifies sensitive data or intellectual property
- Amazon Inspector
    - automated security assessment service, auto assesses apps for vulnerabilities
- Amazon Detective
    - detect root cause of your security issues
    - auto collects log data from other AWS services


# Management & Governance Services

- AWS SSM (Systems Manager)
    - a suite of services that allows us to manager resources and control both AWS cloud and on-prem infra
- AWS Config
    - assess, audit, evaluate configs of your AWS resources
- AWS Organizations
    - consolidate and centrally manage multi AWS accounts
- AWS Service Catalog
    - set up and centrally manage catalogs of approved IT services
- AWS Control Tower
    - set up and govern a secure multi-account AWS environment