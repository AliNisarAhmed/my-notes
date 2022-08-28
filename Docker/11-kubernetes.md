# Kubernetes

Kubernetes is the whole orchestration system, while `kubectl` is the CLI to configure Kubernetes and manager apps


## Definitions

Node: 
    - Single server in the K8s cluster
Kubelet: 
    - Kubernetes agent running on nodes
Control Plane: 
    - Set of containers that manager the cluster
    - Includes API server, scheduler, controller manager, etcd and more
    - Sometimes called the master

![374acdc86103dbcb9d0d9da08a6aea33.png](../images/374acdc86103dbcb9d0d9da08a6aea33.png)


### K8s container abstractions

Pod: 
    - One or more containers running together on one Node
    - Basic unit of deployment. Containers are always in pods
Controller: 
    - For creating/updating/controlling pods and other "objects"
    - Many type of controllers including Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob etc
Service
    - network endpoint to connect to a pod or set of pods at a specific DNS name and port
Namespace: 
    - Filtered group of objects in cluster
    - Not a security feature, not comparable to namespaces in Docker

![3648965233870b60731f5ad4bce8e7d0.png](../images/3648965233870b60731f5ad4bce8e7d0.png)


## Commands

`kubectl run`
    - create single pods

`kubectl create`
    - this command is now used to create deployment
    - `kubectl create deployment my-apache --image httpd`

`kubectl delete deployment my-apache`

`kubectl get all`
 
 `kubectl scale deploy/my-acpache --replicas 2`
 
 `kubectl logs deployment/my-apache`
 
 
 `kubectl describe pod/my-apache-6f45bc5bd9-fj2bl`
 
 ---
 
 
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