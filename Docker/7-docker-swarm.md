# Swarm mode - Built in orchestration

Swarm mode is a clustering solution build inside Docker

- Not enabled by default

`docker run` command does not know how to scale the containers, and it can only start one container.

Introduces these new commands once enabled
- `docker swarm`
- `docker node`
- `docker service`
- `docker stack`
- `docker secret`


![c03447a0290cdca5c0c9d058ba66e6fe.png](../images/c03447a0290cdca5c0c9d058ba66e6fe.png)

Manager nodes have a db locally, called a raft db. It stores their config, and gives them all the info to be the managers.

![12ae16977f187fea4b1b7b9e0ce9c267.png](../images/12ae16977f187fea4b1b7b9e0ce9c267.png)

![614f2a8b5f201f1f1f0266de2547bcbf.png](../images/614f2a8b5f201f1f1f0266de2547bcbf.png)

## Initializing Swarm mode

`docker swarm init`

`docker node ls`

`docker service` command replaces the `docker run` command for a swarm
- `docker service create alpine ping 8.8.8.8`
- `docker service ls`
- `docker service ps <service_name>`
- `docker service update <service_name> --replicas 3` - update the service to initialize 3 replicas
- `docker service rm <service_name>`

## Creating a 3-node swarm cluster

- `docker swarm init --advertise-addr <ip_address>`
- `docker swarm join <token>` - this command is the output of the command above, we run it in the two nodes we want to be the workers
- workers cannot run swarm commands like `docker node ls`
- `docker node update --role manager node2`
- `docker node ls`
- `docker swarm join-token manager` - to get the join token which will enable a node to join as a manager
- `docker service create --replicas 3 alpine ping 8.8.8.8`
- `docker node ps <node_name>`
- `docker service ps <service_name>`

Once we have a 3-node swarm, we can run most of our commands from the manager node (instead of going to each node to run the commands)