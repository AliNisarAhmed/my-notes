# Docker - How it works 

## Docker Images vs Containers

![eefa870ee8d7502a8f10b088694fb039.png](eefa870ee8d7502a8f10b088694fb039.png)

## How docker works 

Normal computer processes 

![7461844a1884a42126ee837d04a93836.png](7461844a1884a42126ee837d04a93836.png)

Some ways OS can isolate resources required by processes: 

![ac96f4f5e5fcea131533bdbf844850b9.png](ac96f4f5e5fcea131533bdbf844850b9.png)


With Docker: 

![b2297f1860df1bf9f96745b05deca6e8.png](b2297f1860df1bf9f96745b05deca6e8.png)

An Image is a copy/paste of the FileSystem required to run the containers of that images, as shown below: 

![959903c516519d97412061c71d9a6dce.png](959903c516519d97412061c71d9a6dce.png)

Docker runs on Linux, on Windows/MacOS it runs a Linux VM to do so: 

![8176433684c4a7164734d6f3261aa19a.png](8176433684c4a7164734d6f3261aa19a.png)


---

### Sample Docker file and how docker executes it 
# Use an existing docker image as a base 

FROM alpine


# Download and install a dependency

```
FROM alpine

RUN apk add --update redis

# Tell the image what to do when it starts as a container

CMD ["redis-server"]

```
![66cced45157e0a741eaa75720e50c9e6.png](66cced45157e0a741eaa75720e50c9e6.png)
