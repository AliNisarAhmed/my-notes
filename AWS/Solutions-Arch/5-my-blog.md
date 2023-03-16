# Design MyBlog.com

1. The website should scale globally
2. Blogs are rarely written, but often read
3. Some of the website is purely static files, the rest is a dynamic REST API
4. Caching must be implemented where posssible
5. Any new users that subscribes should receive a welcome email
6. Any photo uploaded to the blog should have a thumbnail generated


## Solution

![2d3008eb621c6410947ed5f2fd8d683b.png](../../images/2d3008eb621c6410947ed5f2fd8d683b.png)


![c3096401fca3605f309447e83fa231ad.png](../../images/c3096401fca3605f309447e83fa231ad.png)

^ In the above we are using DynamoDB Global tables

![bc6e7f6ae226f4c409fe97e920c6cb1f.png](../../images/bc6e7f6ae226f4c409fe97e920c6cb1f.png)

![2d93a4953dd770904d5c7c4bdae2d902.png](../../images/2d93a4953dd770904d5c7c4bdae2d902.png)



- serve static content using CloudFront with S3
- the REST API was serverless, did not need Cognito because public
- We leveraged a Global DynamoDB table to serve the data globally
    - we could also have used Aurora Global Database, but that is not serverless
- We enabled DynamoDB streams to trigger a Lambda function
- the lambda function had an IAM role which could use SES
- SES (Simple Email service) was used to send emails in a serverless way
- S3 can trigger SQS/SNS/Lambda to notify of events