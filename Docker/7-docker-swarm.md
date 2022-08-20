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

---

## Scaling out with "Overlay" networking

Once we have our services started, we want them to communicate to each other. 

One such way is to use a builtin network driver called `overlay`
- it creates a swarm wide bridge network, and then the nodes communicate with each other

Overlay network is for container-to-container traffic inside a single swarm.

Each service in a swarm can be connected to multiple networks.
    - e.g. front-end, backend

Running a postgres + Drupal combo on a 3 node service:
To create we use `--driver overlay` option when creating a network
- `docker network create --driver overlay <network_name>`

- `docker service create --name psql --network mydrupal postgres` 
- `docker service create --name drupal --network mydrupal -p 80:80 drupal`
- `docker service ps drupal`
- each of these services will be started on a random node
- they communicate over the overlay network
- once drupal is set up, we can use the IP of any of the node (find the IP with `ip addr`) to access our drupal site. This is enabled by the feature "Swarm Routing Mesh"
- While setting up the drupal, we can use `psql` name as a DNS for the postgres database instead of the IP.

### Swarm Routing Mesh

Routing Mesh is in incoming/ingress network that routes packers for a Service to proper Task for that Service

Spans all nodes in the swarm.

Uses IPVS from Linux Kernel

Load balances Swarm Services across their Tasks

Two ways Routing Mesh works: 
- Container-to-Container in a Overlay network (uses VIP: Virtual IP)
    - Virtual IP is a private IP that swarm puts in front of all the nodes
- External traffic incoming to published ports is routed to a proper container (so we do not need to care which IP and the node the container is running on)

![566ecc0369db183a88c99daffbec2de3.png](../images/566ecc0369db183a88c99daffbec2de3.png)

![78919d9c7ca21cd0c5c022b85a02cdda.png](../images/78919d9c7ca21cd0c5c022b85a02cdda.png)

To see the Routing mesh in action, below we create an ElasticSearch Service with 3 replicas
- `docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2`
- `docker service ps search` - we can see that 3 tasks were created on the 3 different nodes
- `curl localhost:9200` - if we do this command multiple times, we will see the `name` property change, showing the names of the service on each node

Routing Mesh
- is stateless load balancing 
    - this means, No Session cookies, and No client can consistently talk to a single container
- acts as the OSI Layer 3 LB (TCP), not Layer 4 (DNS)

To solve both these problems, use nginx, HAProxy

### Assignment

We create a voting app, represented by the diagram below: 

![architecture](../images/architecture.png)

Assuming we already have a 3-node swarm running:
- `docker network create -d overlay frontend`
- `docker network create -d overlay backend`

- `docker service create --name vote --network frontend -p 80:80 --replicas 2 bretfisher/examplevotingapp_vote`

- `docker service create --name redis --network frontend redis:3.2`

- `docker service create --name worker --network frontend --network backend bretfisher/examplevotingapp_worker`

- `docker service create --name db --network backend -e POSTGRES_HOST_AUTH_METHOD=trust --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4`

- `docker service create --name result --network backend -p 5001:80 bretfisher/examplevotingapp_result`