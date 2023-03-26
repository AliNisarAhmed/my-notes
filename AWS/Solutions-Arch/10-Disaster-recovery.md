# Disaster Recovery in AWS


## RPO & RTO


**Recovery Point Objective (RPO)** is a measure of how frequently you take backups. If a disaster occurs between backups, can you afford to lose five minutes’ worth of data updates? Or five hours? Or a full day? RPO represents how fresh recovered data will be. In practice, the RPO indicates the amount of data (updated or created) that will be lost or need to be reentered after an outage.

**Recovery Time Objective (RTO) **is the amount of downtime a business can tolerate. In a high-frequency transaction environment, seconds of being offline can represent thousands of dollars in lost revenue, while other systems (such as HR databases) can be down for hours without adversely impacting the business. The RTO answers the question, “How long can it take for our system to recover after we were notified of a business disruption?”

![fe5dd973bf7026d360477ea22fa87ea4.png](../../images/fe5dd973bf7026d360477ea22fa87ea4.png)



## Disaster Recovery Strategies

1. Backup And Restore
2. Pilot Light
3. Warm Standby
4. Hot Site / Multi Site Approach

![c5e6565a361ef4150e582ed4bb85ed6a.png](../../images/c5e6565a361ef4150e582ed4bb85ed6a.png)



### Backup & Restore

![a216cbd038f1442f369ac66c6dc36574.png](../../images/a216cbd038f1442f369ac66c6dc36574.png)

We take regular backups of our on-prem data, (e.g. every 24 hours), thus high RPO (meaning more data loss till recovery), and high RTO (will take time to restore all the backups)

### Pilot Light


![838921182618c734862e34b917e57668.png](../../images/838921182618c734862e34b917e57668.png)


### Warm Standby

![56f4f3d75b148ee37f3e88c4412f85a9.png](../../images/56f4f3d75b148ee37f3e88c4412f85a9.png)


### Multi-site / Hot site approach

![5b1479a4cb9fd8693de3f5a975a88b83.png](../../images/5b1479a4cb9fd8693de3f5a975a88b83.png)



## Disaster Recovery Tips

![fd205b13599bae52908bb85a2dcbf2f8.png](../../images/fd205b13599bae52908bb85a2dcbf2f8.png)


---


# Transferring large amounts of data into AWS

![566499803f535809738cc975a4ffef64.png](../../images/566499803f535809738cc975a4ffef64.png)

