# Design MyTodoList

- A Mobile app
- Serverless
- Users should be able to directly interact with their own S3 folder
- Users should authenticate through a managed serverless service
- The users can write and read to-dos, but they mostly read them
- The DB should scale, and have some high read throughput


## Solution


![1c7236f04765a6df44cea256d11505d1.png](../../images/1c7236f04765a6df44cea256d11505d1.png)


AWS STS (Security Token Service) that enables you to request temporary, limited-privilege creds for users


![5625e71e97df96ca9d958027ae8057b2.png](../../images/5625e71e97df96ca9d958027ae8057b2.png)


Using Cognito to generate temporary creds with STS to access S3 bucket with restricted policy.

![1e7fafe867e9b024f7aecbaaf86545ac.png](../../images/1e7fafe867e9b024f7aecbaaf86545ac.png)


We can cache REST requests at API Gateway layer

We can cache the reads on DynamoDB using DAX

![84081983b2fbc25966b7864cf85b442c.png](../../images/84081983b2fbc25966b7864cf85b442c.png)

