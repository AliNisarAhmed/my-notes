# Aurora

![1311baf570103df8ee6c92a851eb597a.png](1311baf570103df8ee6c92a851eb597a.png)

![de80b1bb1dd7fe97863593b306b53c4c.png](de80b1bb1dd7fe97863593b306b53c4c.png)

![2d2b7f5d2cd0a20e1cc0a015d1075a36.png](2d2b7f5d2cd0a20e1cc0a015d1075a36.png)

## Features

- Automatic Fail-over
- Backup and Recovery
- Isolation & Security
- Industy Compliance
- Push-button scaling 
- Automated Patching with Zero downtime 
- Advanced Monitoring 
- Routing Maintenance 
- Backtrack: restore data at any point of time without using backups

### Security

Similar to RDS because they use the same engines

- Encryption at rest using KMS 
- Automated backups, snapshots and replicas are also encrypted
- Encryption in flight using SSL (same process as MySQL or Postgres)
- Possibility to authenticate using IAM token (same method as RDS)
- You are responsible for protecting the instance with Security groups.
- You can't SSH

## Aurora Serverless

![c656cdff2fcfb631efc96604a3d3e7fb.png](c656cdff2fcfb631efc96604a3d3e7fb.png)

Aurora Serverless can be used by a development team to minimize costs.

## Aurora Multi-Master

![8dae5d3bec6d786e18304706bb33c077.png](8dae5d3bec6d786e18304706bb33c077.png)


## Aurora Global Database

Global Databases allow you to have cross region (note: cross-region, not cross AZ) replication