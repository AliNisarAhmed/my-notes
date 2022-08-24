# Why do we need Orchestration

Orchestration provides answers to the following questions: 
- How do we automate container lifecycle?
- HOw can we easily scale out/in/up/down?
- How can we ensure our containers are re-created if they fail?
- How can we replace containers without downtime? (blue/green deploy)
- How can we control/track where containers get started?
- How can we create cross-node virtual networks
- How can we ensure only trusted servers run our containers?
- How can we store secrets, keys, passwords and get them to the right container (and only that container)?


# Kubernetes

Kubernetes is a popular container orchestration
            
Runs on top of Docker (usually) as a set of APIs in containers

Provide API/CLI to manage containers across servers

Many clouds provide it for us, and many vendors make a "distribution" of it


## Why Kubernetes?

Not every solution needs Kubernetes, especially small teams.

The rule of thumb is:

Number of Servers + Change Rate of Application = Benefit of Orchestration

As the left side of the above equation goes up, so does the benefit of orchestration increase.

Then decide which orchestrator.

If Kubernetes, decide which distribution: 
- cloud, or self managed (Docker Enterprise, Rancher, OpenShift, Canonical, VMWare PKS)


# Kubernetes vs Swarm

Advantages of Swarm: 
- comes with Docker, single vendor container platform
- Easiest orchestrator to deploy/manage yourself
- Follows 80/20 rule, 20% of features for 80% of use cases
- Runs anywhere Docker runs
    - local, cloud, datacenter
    - ARM, Windows, 32 bit, 64 bit
- Secure by default
- Easier to troubleshoot

Advantages of Kubernetes: 
- Clouds will deploy/manage kubernetes for you
- Infrastructure vendors often provide their own distributions with more support
- Widest adaption and community
- Flexible: covers widest set of use cases
- "Kubernetes" first vendor support
- Trendy, will benefit your career