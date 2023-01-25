# AWS Lambda

Synchronous Invocation: When the client waits for and expects a response
Asynchronous Invocation: When the client does not wait for the response
    - Uses an internal event queue to trigger the function


## Lambda function Execution environment lifecycle

1. Init
2. Invoke
3. Shutdown

![b5b0dbcd9d363b2a66e4403a9b424387.png](../../../../dev/my-notes-work/images/b5b0dbcd9d363b2a66e4403a9b424387.png)


![02f944231a185b9bcff11f0564c50057.png](../../../../dev/my-notes-work/images/02f944231a185b9bcff11f0564c50057.png)


![b48756bf317dced33b2c7f1679084355.png](../../../../dev/my-notes-work/images/b48756bf317dced33b2c7f1679084355.png)

Lambda Layers makes external dependencies shareable

![5594f852f6d63e902623ef9d132337ec.png](../../../../dev/my-notes-work/images/5594f852f6d63e902623ef9d132337ec.png)

![02707cff2b5df1e79385c5f20240b486.png](../../../../dev/my-notes-work/images/02707cff2b5df1e79385c5f20240b486.png)


- Amazon Lambda by default cannot connect to private subnet on a VPC
- To Connect:
    - AWS Lambda needs policy `AWSLambdaVPCAccessExecutionRole`
    - This allows Lambda to create Elastic Network interface (ENI) in private subnet
    - Once connected to private subnet, the Lambda loses conncetion to public internet
    - to restore it, use a NAT gatway in public subnet, and have the route table of private subnet point to it

![c7d86091d21b3e07cc153bbbced349e7.png](../../../../dev/my-notes-work/images/c7d86091d21b3e07cc153bbbced349e7.png)


Versions and Aliases
- Every time a Lambda function is deployed, it is given a new version number which is appended to its name
- To avoid frequently changing function name, we can attach aliases to a particular version, and only update alias to point to a new version when we actually want to use that version
