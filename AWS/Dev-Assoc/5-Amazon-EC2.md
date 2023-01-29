# Amazon EC2

Amazon's Virtual Machine service

## Amazon Machine Image (AMI)

User can launch a pre-configured EC2 Instance

AMIs are regional in scope and cannot be moved b/w regions, but can be copied to other regions


![1ea87cdcd7974e7147227504c3e99ea0.png](../../../../dev/my-notes-work/images/1ea87cdcd7974e7147227504c3e99ea0.png)


## User Data

Set of commands or script run at the launch of EC2 instance

Helpful when we have multiple EC2 instances connected to Amazon EFS or Auto-Scaling Group

User data must be in base-64 encoded, limited to 16 KB

can be retrieved using ttp://169.254.169.254/latest/user-data