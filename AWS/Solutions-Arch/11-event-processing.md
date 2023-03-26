# Event Processing in AWS

## Lambda, SNS & SQS

![16185b81caa9fb49878466c9f40f314d.png](../../images/16185b81caa9fb49878466c9f40f314d.png)


1. SQS + Lambda
    - Lambda pulls the SQS queue
    - Lambda retries failed messages, and after multiple tries (e.g. 5), sends it to DLQ (Dead Letter Queue)
2. SQS Fifo + Lambda
    - Messages will be processed in order
    - Without DLQ, there is a risk of a failed message blocking the whole queue
3. SNS + Lambda
    - Message sent async to Lambda
    - because of async, DLQ needs to be set up on the Lambda side (rather than SNS)



## Fan out Patterns

![5623797b2ed004d69bd8a849a0f97ce0.png](../../images/5623797b2ed004d69bd8a849a0f97ce0.png)



## S3 Event Notifications


![9b9b992150202a13b6191e67c000d68e.png](../../images/9b9b992150202a13b6191e67c000d68e.png)


## S3 Event Notifications using EventBridge

![82731e441e7a526b2c7e4c8c056f4d18.png](../../images/82731e441e7a526b2c7e4c8c056f4d18.png)



## Amazon EventBridge - Alert on API Calls

![fd12e72f38f129244b2efa41bdc0f41d.png](../../images/fd12e72f38f129244b2efa41bdc0f41d.png)

