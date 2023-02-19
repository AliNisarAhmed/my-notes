# AWS CloudTrail

A managed service to monitor and log account activities from iam user, iam role, aws services or accounts

AWS Management Console, API, or CLI actions are all recorded as events

It can help you facilitate **governance**, **compliance**, **operational auditing** and **risk auditing** of your AWS account

### CloudTrail Event Types

1. Management Events
    - management operation events such as user logins
    - enabled by default
2. Data events
    - S3 Object-level API events (ex. `GetObject`, `PutObject`)
    - Lambda function Invoke API
    - Additional charges apply for these events
3. Insight events
    - unusual write API calls


CloudTrail logs can be stored in AmazonS3

However, even without CloudTrail Events, we can monitor account activities by using **Event History** for the past 90 days


# CloudWatch

CloudWatch purpose:
- Collect
    - Collect metrics and logs from your AWS resources, apps, and on-prem services
- Monitor
    - Visualize your operational data using dashboards and set up alerts
- Act
    - Automate the process of responding to operational changes and alarms
    - trigger actions like Auto Scaling, stopping instances to reduce overages, and trigger other workflows with services like Lambda, SNS and CloudFormation
- Analyze
    - Extended data retention up to 15 months with real-time analytics


CloudWatch Services
- Metrics
    - Most AWS services send metric data to CloudWatch every one minute
    - EC2 instances send metrics data to CloudWatch every 5 minutes by default but you can also enable detailed monitoring to send metric data at a 1-minute interval
    - You can also publish your own custom metrics
    - Metric data in Cloudwatch is kept for 15 months
- Logs
    - monitor store and access log files from EC2, CloudTrail, Route53, on-prem servers and other resources
    - 3 types of logs
        1. Vended
            - natively published by AWS services on behalf of the customer
            - currently VPC and Route53
        2. Published by AWS
            - currently 30 services that publish these logs
        3. logs from on-prem services
            - CloudWatch Agent or PutLogData API
- Alarms
    - triggers one or more actions based on the value of the metric relative to the threshold you set
    - Actions can be in the form of:
        - sending notification to an Amazon SNS topic
        - performing an Amazon EC2 action
        - scaling an Auto Scaling group
        - creating an incident in Systems Manager
        - sending a billing alert
        - invoking a Lambda function
- Events
    - provides near real-time stream of system events that describe changes in your AWS resources
    - this is done by creating event rules that matches events that you want to monitor and route them to one or more targets
    - Targets
        - EC2 instance action
        - triggering a Lambda function
        - invoking ECS task
        - Setting off a Systems Manager Automation doc
        - publishing a message to SNS topic
    - Once a rule is set up, CloudWatch Events becomes aware of operational changes once they occur
    - CloudWatch Events and Amazon EventBridge are same and have same API Infra, Amazon EventBridge is the newer one
- Dashboard
    - reusable graphs and visualize your resources


Difference between Alarm and Events
- Alarms take action when a metric reaches a certain threshold
- Events react to real-time system events performed on your AWS resources


# AWS X-ray

A distributed tracing service that helps you analyze and debug potential issues in a distributed application

Can help you pinpoint which part causes the performance bottleneck in your application

The service map feature gives you visibility on the user requests as they travel through the components of your app

The end to end tracing feature tracks the path of an individual request as it passes through each service node

Supports apps running on EC2, ECS, ELB and Lambda