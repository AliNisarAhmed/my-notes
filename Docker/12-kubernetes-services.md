# K8s Services

## Exposing Pods with Services

`kubectl expose` command creates a **service** for existing pods

A service is a stable address for pod(s)
- an endpoint that is consistent so that other things within the cluster can access other things using it

If we want to connect to pod(s), we need a service.

CoreDNS allows us to resolve services by name.

There are different types of Services
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName

ClusterIP
- is the default
- its only available within the cluster, for one set of pods talking to another set of pods.
- gets single internal virtual IP address by the DNS system
- Pods can reach service on apps port number
- A pod can only communicate using ClusterIP
- A clusterIP is ONLY good within the cluster

NodePort
- High port allocated on each node (i.e. port number > 30000)
- Port is open on every node's IP
- Anyone can connect (if they can reach the node)
- Other pods need to be updated to this port

LoadBalancer
- Used mostly in cloud
- Controls a LB endpoint external to the cluster (e.g. AWS LB)
- Only available when infra provider gives you a LB (AWS ELB)
- Creates NodePort+ClusterIP services, tells LB to send to NodePort
- Only for traffice coming in to our services from external source, & requires an external infra structure provider

ExternalName
- This is about stuff in our cluster needs to talk to outside services
- Adds CNAME DNS record to CoreDNS only
- Not used for Pods, but for giving pods a DNS name to use for something outside K8s

### Creating Services

##### Creating a ClusterIP Service

- Open two terminals, in one run `kubectl get pods -w` to watch as the pods are created
- Create a deployment
    - `kubectl create deployment httpenv --image bretfisher/httpenv`
- Scale the deployment
    - `kubectl scale deployment/httpenv --replicas=5`
- Create a ClusterIP service (default)
    - `kubectl expose deployment/httpenv --port 8888`
- Check the new service with, can also check its allocated IP
    - `kubectl get service`
- but how do we curl to the new service
    - If on Docker desktop, run another pod running on the same cluster, so we can internally curl 
    - `kubectl run tmp-shell --rm -it --image bretfisher/netshoot -- bash`
    - Once in, we can then simply do `curl httpenv:8888` to reach our service
    - On linux, it may be possible to `curl <ip_address>` of the ClusterIP Service, but it did not work for me


##### Creating a NodePort Service

- Lets expose a NodePort so we can access it via the host IP (including localhost on Linux/Mac/Wind.)
    - `kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort`
    - Its usually assigned a port like `8888:32334/TCP`, 32334 is the port that we can reach it on
    - so, if we do curl `localhost:32334`, we should be able to reach the nodePort
    - Creating a NodePort also creates a ClusterIP
- These 3 services types are additive, meaning, each one creates the one above it: 
    - ClusterIP
    - NodePort
    - LoadBalancer

##### Create a Load Balancer Service

Docker Desktop provides a built-in LB

- `kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer`
- We can reach the LB by `curl localhost:8888`
    - An LB is usually listed with ports `8888:32024`, but the `32024` part is the underlying NodePort used by the LB, the LB is actually running on `8888`

##### Clean up

`kubectl delete service/httpenv service/httpenv-np service/httpenv-lb deployment/httpenv`


## Kubernetes Services DNS

Internel DNS is provided by [CoreDNS](https://github.com/coredns/coredns)

Like swarm, this is DNS-based service discovery (i.e. use a service name to talk to it, instead of remembering its IP)

So far, we have been using hostnames to access services `> curl <hostname>`, but that only works for services in the same namespaces

Servies also have a FQDN
- `> curl <hostname>.<namespace>.svc.cluster.local`