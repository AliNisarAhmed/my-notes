# Azure

## Module 1

### Cloud Computing

- Cloud computing is renting resources, like storage space or CPU cycles, on another company's computers. You only pay for what you use. The company providing these services is referred to as a cloud provider. Some example providers are Microsoft, Amazon, and Google.

- The cloud provider is responsible for the physical hardware required to execute your work, and for keeping it up-to-date. The computing services offered tend to vary by cloud provider. However, typically they include:

  - Compute power - such as Linux servers or web applications
  - Storage - such as files and databases
  - Networking - such as secure connections between the cloud provider and your company
  - Analytics - such as visualizing telemetry and performance data

![Computing](../images/computing-cloud.PNG)

### Benefits of Clouds Computing

- Cost Effective
  - Pay as you go OR consumption based
  - No upfront indrastructure costs
- Scalable

  - Vertical Scaling - is the process of adding resources to increase the power of an existing server. Some examples of vertical scaling are: adding more CPUs, or adding more memory.
  - Horizontal scaling, also known as "scaling out", is the process of adding more servers that function together as one unit. For example, you have more than one server processing incoming requests.
  - Scaling can be done manually or automatically based on specific triggers such as CPU utilization or the number of requests and resources that can be allocated or de-allocated in minutes.

- Elastic
  - As your workload changes due to a spike or drop in demand, a cloud computing system can compensate by automatically adding or removing resources.
- Current
- Reliable
- Global
- Secure

### Compliance Terms

When selecting a cloud provider to host your solutions, you should understand how that provider can help you comply with regulations and standards. Some questions to ask about a potential provider include:

- How compliant is the cloud provider when it comes to handling sensitive data?
- How compliant are the services offered by the cloud provider?
- How can I deploy my own cloud-based solutions to scenarios that have accreditation or compliance requirements?
- What terms are part of the privacy statement for the provider?

#### Compliance Offerings

- Criminal Justice Information Services (CJIS)
- Cloud Security Alliance (CSA) STAR Certification.
- General Data Protection Regulation (GDPR)
- EU Model Clauses
- Health Insurance Portability and Accountability Act (HIPAA)
- International Organization for Standardization (ISO) and the International Electrotechnical Commission (IEC)
- Multi-Tier Cloud Security (MTCS) Singapore
- Service Organization Controls (SOC) 1, 2, and 3.
- National Institute of Standards and Technology (NIST) Cybersecurity Framework (CSF).
- UK Government G-Cloud

### CapEx vs OpEx Computing Costs

Capex

- Server Costs (Hardware)
- Storage Costs
- Network Costs
- Backup and Archive Costs
- Organization continuity and disaster recovery costs
- Datacenter infrastructure costs
- Technical Personals

OpEx

- Leasing software cost and customized features
- Scaling charges based on usage
- Billing at the user or org level

### Cloud Deployment models

#### Public

- In this case, you have no local hardware to manage or keep up-to-date – everything runs on your cloud provider's hardware. In some cases, you can save additional costs by sharing computing resources with other cloud users.
- Businesses can use multiple public cloud providers of varying scale. Microsoft Azure is an example of a public cloud provider.
- Advantages

  - High scalability/agility – you don't have to buy a new server in order to scale
  - Pay-as-you-go pricing – you pay only for what you use, no CapEx costs
  - You're not responsible for maintenance or updates of the hardware
  - Minimal technical knowledge to set updates and use - you can leverage the skills and expertise of the cloud provider to ensure workloads are secure, safe, and highly available

- Disadvantages

  Not all scenarios fit the public cloud. Here are some disadvantages to think about:

  - There may be specific security requirements that cannot be met by using public cloud
  - There may be government policies, industry standards, or legal requirements which public clouds cannot meet
  - You don't own the hardware or services and cannot manage them as you may want to
  - Unique business requirements, such as having to maintain a legacy application might be hard to meet

#### Private Cloud

- In a private cloud, you create a cloud environment in your own datacenter and provide self-service access to compute resources to users in your organization. This offers a simulation of a public cloud to your users, but you remain completely responsible for the purchase and maintenance of the hardware and software services you provide.

- Advantages

This approach has several advantages:

- You can ensure the configuration can support any scenario or legacy application
- You have control (and responsibility) over security
- Private clouds can meet strict security, compliance, or legal requirements

- Disadvantages

Some reasons teams move away from the private cloud are:

- You have some initial CapEx costs and must purchase the hardware for startup and maintenance
- Owning the equipment limits the agility - to scale you must buy, install, and setup new hardware
- Private clouds require IT skills and expertise that's hard to come by

#### Hybrid Cloud

- A hybrid cloud combines public and private clouds, allowing you to run your applications in the most appropriate location. For example, you could host a website in the public cloud and link it to a highly secure database hosted in your private cloud (or on-premises datacenter).
- This is helpful when you have some things that cannot be put in the cloud, maybe for legal reasons. For example, you may have some specific pieces of data that cannot be exposed publicly (such as medical data) which needs to be held in your private datacenter. Another example is one or more applications that run on old hardware that can't be updated. In this case, you can keep the old system running locally, and connect it to the public cloud for authorization or storage.

- Advantages

Some advantages of a hybrid cloud are:

- You can keep any systems running and accessible that use out-of-date hardware or an out-of-date operating system
- You have flexibility with what you run locally versus in the cloud
- You can take advantage of economies of scale from public cloud providers for services and resources where it's- cheaper, and then supplement with your own equipment when it's not
- You can use your own equipment to meet security, compliance, or legacy scenarios where you need to completely control the environment

Disadvantages

Some concerns you'll need to watch out for are:

- It can be more expensive than selecting one deployment model since it involves some CapEx cost up front
- It can be more complicated to set up and manage

### Types of Cloud Services

#### IaaS

- Infrastructure as a Service is the most flexible category of cloud services. It aims to give you the most control over the provided hardware that runs your application (IT infrastructure servers and virtual machines (VMs), storage, and operating systems). Instead of buying hardware, with IaaS, you rent it. It's an instant computing infrastructure, provisioned and managed over the internet.
- When using IaaS, ensuring that a service is up and running is a shared responsibility: the cloud provider is responsible for ensuring the cloud infrastructure is functioning correctly; the cloud customer is responsible for ensuring the service they are using is configured correctly, is up to date, and is available to their customers. This is referred to as the **shared responsibility model**.

#### PaaS

- PaaS provides an environment for building, testing, and deploying software applications. The goal of PaaS is to help you create an application quickly without managing the underlying infrastructure. For example, when deploying a web application using PaaS, you don't have to install an operating system, web server, or even system updates.

#### SaaS

- SaaS is software that is centrally hosted and managed for the end customer. It is usually based on an architecture where one version of the application is used for all customers, and licensed through a monthly or annual subscription. Office 365, Skype, and Dynamics CRM Online are perfect examples of SaaS software.

![Cloud Types](../images/cloud-types.PNG)

---

## Module 2

### Azure Billing

- With Azure, you only pay for what you use. You'll receive a monthly invoice with payment instructions provided. You may organize your invoice into line items that make sense to you and meet your budget and cost tracking needs. You also can get set up for multiple invoices.

#### Azure subscription

- When you sign up, an Azure subscription is created by default.
- An Azure subscription is a logical container used to provision resources in Azure. It holds the details of all your resources like virtual machines (VMs), databases, and more.
- When you create an Azure resource like a VM, you identify the subscription it belongs to. - As you use the VM, the usage of the VM is aggregated and billed monthly.

#### Create additional Azure subscriptions

- Environments: When managing your resources, you can choose to create subscriptions to set up separate environments for development and testing, security, or to isolate data for compliance reasons. This is particularly useful because resource access control occurs at the subscription level.
- Organizational structures: You can create subscriptions to reflect different organizational structures. For example, you could limit a team to lower-cost resources, while allowing the IT department a full range. This design allows you to manage and control access to the resources that users provision within each subscription.
- Billing: You might want to also create additional subscriptions for billing purposes. Because costs are first aggregated at the subscription level, you might want to create subscriptions to manage and track costs based on your needs. For instance, you might want to create a subscription for your production workloads and another subscription for your development and testing workloads.
- Subscription limits: Subscriptions are bound to some hard limitations. For example, the maximum number of Express Route circuits per subscription is 10. Those limits should be considered as you create subscriptions on your account. If there is a need to go over those limits in particular scenarios, then you might need additional subscriptions.

![Billing Structure](../images/billing-structure.PNG)

### Azure Support Plans

![Support plans](../images/azure-support.PNG)

---

## Module 4

### Data Centers

- Microsoft Azure is made up of datacenters located around the globe. When you leverage a service or create a resource such as a SQL database or virtual machine, you are using physical equipment in one or more of these locations.

- The specific datacenters aren't exposed to end users directly; instead, Azure organizes them into regions.

### Regions

- A region is a geographical area on the planet containing at least one, but potentially multiple datacenters that are nearby and networked together with a low-latency network. Azure intelligently assigns and controls the resources within each region to ensure workloads are appropriately balanced.

### Geographies

- Azure divides the world into geographies that are defined by geopolitical boundaries or country borders.
- An Azure geography is a discrete market typically containing two or more regions that preserve data residency and compliance boundaries.

Geographies are broken up into the following areas:

- Americas
- Europe
- Asia Pacific
- Middle East and Africa

### Availability Zones

Availability Zones are physically separate datacenters within an Azure region.

Each Availability Zone is made up of one or more datacenters equipped with independent power, cooling, and networking. It is set up to be an isolation boundary. If one zone goes down, the other continues working. Availability Zones are connected through high-speed, private fiber-optic networks.

You can use Availability Zones to run mission-critical applications and build high-availability into your application architecture by co-locating your compute, storage, networking, and data resources within a zone and replicating in other zones. Keep in mind that there could be a cost to duplicating your services and transferring data between zones.

Availability Zones are primarily for VMs, managed disks, load balancers, and SQL databases. Azure services that support Availability Zones fall into two categories:

- Zonal services – you pin the resource to a specific zone (for example - virtual machines, managed disks, IP addresses)
- Zone-redundant services – platform replicates automatically across zones (for example, zone-redundant storage, SQL Database).

### Region Pairs

- Availability zones are created using one or more datacenters, and there is a minimum of three zones within a single region. However, it's possible that a large enough disaster could cause an outage large enough to affect even two datacenters. That's why Azure also creates region pairs.

- Each Azure region is always paired with another region within the same geography (such as US, Europe, or Asia) at least 300 miles away. This approach allows for the replication of resources (such as virtual machine storage) across a geography that helps reduce the likelihood of interruptions due to events such as natural disasters, civil unrest, power outages, or physical network outages affecting both regions at once. If a region in a pair was affected by a natural disaster, for instance, services would automatically fail over to the other region in its region pair.

= Additional advantages of region pairs include:

- If there's an extensive Azure outage, one region out of every pair is prioritized to make sure at least one is restored as quick as possible for applications hosted in that region pair.
- Planned Azure updates are rolled out to paired regions one region at a time to minimize downtime and risk of application outage.
- Data continues to reside within the same geography as its pair (except for Brazil South) for tax and law enforcement jurisdiction purposes.

### Service Level Agreements (SLAs)

Formal documents called Service-Level Agreements (SLAs) capture the specific terms that define the performance standards that apply to Azure.

- SLAs describe Microsoft's commitment to providing Azure customers with specific performance standards.
- There are SLAs for individual Azure products and services.
- SLAs also specify what happens if a service or product fails to perform to a governing SLA's specification.

There are three key characteristics of SLAs for Azure products and services:

- Performance Targets
- Uptime and Connectivity Guarantees
- Service credits

A typical SLA specifies performance-target commitments that range from 99.9 percent ("three nines") to 99.999 percent ("five nines"), for each corresponding Azure product or service. These targets can apply to such performance criteria as uptime or response times for services.

Service credits (discounts) is given if a performance taget is not met.

### Composite SLAs

- When combining SLAs across different service offerings, the resultant SLA is called a Composite SLA. The resulting composite SLA can provide higher or lower uptime values, depending on your application architecture.

- Consider a web app (99.95% SLA) which talks to a SQL db (99.99% SLA)
- Composite SLA for this application will be
  - 99.95% x 99.99% = 99.94 %
    -This means the **combined probability** of failure is higher than the individual SLA values. This isn't surprising, because an application that relies on multiple services has more potential failure points.
- We can improve the composite SLA by using, for example, a queue (99.95%) for tasks when the DB is not available
- now the application will fail when both db and queue fail (let say the probability of that is 0.0001 x 0.001)
- The composite SLA for DB + QUEUE will be
  - 1.0 - (0.0001 x 0.001) = 99.99999 %
- The composite SLA for the app would become
  - 99.95% x 99.99999 % = ~99.95 %
- Notice we've improved our SLA behavior. However, there are trade-offs to using this approach: the application logic is more complicated, you are paying more to add the queue support, and there may be data-consistency issues you'll have to deal with due to retry behavior.

### Application SLAs

By creating your own SLAs, you can set performance targets to suit your specific Azure application. This approach is known as an Application SLA.

#### Resiliency

Resiliency is the ability of a system to recover from failures and continue to function. It's not about avoiding failures, but responding to failures in a way that avoids downtime or data loss. The goal of resiliency is to return the application to a fully functioning state following a failure. High availability and disaster recovery are two crucial components of resiliency.

#### Failure Mode Analysis (FMA)

When designing your architecture you need to design for resiliency, and you should perform a **Failure Mode Analysis (FMA)**. The goal of an FMA is to identify possible points of failure and to define how the application will respond to those failures.

#### Availability

Availability refers to the time that a system is functional and working.

Tip: For example: A workload that requires 99.99 percent uptime shouldn't depend upon a service with a 99.9 percent SLA.

Most providers prefer to maximize the availability of their Azure solutions by minimizing downtime. However, as you increase availability, you also increase the cost and complexity of your solution.

#### Considerations for defining application SLAs

- If your application SLA defines four 9's (99.99%) performance targets, recovering from failures by manual intervention may not be enough to fulfill your SLA. Your Azure solution must be self-diagnosing and self-healing instead.
- It is difficult to respond to failures quickly enough to meet SLA performance targets above four 9's.
- Carefully consider the time window against which your application SLA performance targets are measured. The smaller the time window, the tighter the tolerances. If you define your application SLA as hourly or daily uptime, you need to understand these tighter tolerances might not allow for achievable performance targets.

---

## Module 5

- There are Four common techniques for performing compute in Azure:
  1. Virtual Machines
  2. Containers
  3. Azure App Service
  4. Serverless Computing

### Virtual Machines

- Virtual machines, or VMs, are software emulations of physical computers.
- They provide infrastructure as a service (IaaS) in the form of a virtualized server and can be used in many ways. Just like a physical computer, you can customize all of the software running on the VM.

- You can run single VMs for testing, development, or minor tasks; or you can group VMs together to provide high availability, scalability, and redundancy.

#### Availability Sets

- An availability set is a logical grouping of two or more VMs that help keep your application available during planned or unplanned maintenance.
- A **_planned maintenance_** event is when the underlying Azure fabric that hosts VMs is updated by Microsoft.
- When the VM is part of an availability set, the Azure fabric updates are sequenced so not all of the associated VMs are rebooted at the same time. VMs are put into different update domains.
- **_Update domains_** indicate groups of VMs and underlying physical hardware that can be rebooted at the same time. Update domains are a logical part of each data center and are implemented with software and logic.
- **_Unplanned maintenance_** events involve a hardware failure in the data center, such as a power outage or disk failure.
- VMs that are part of an availability set automatically switch to a working physical server so the VM continues to run.
- The group of virtual machines that share common hardware are in the same **_fault domain_**.
- A fault domain is essentially a rack of servers.

![Example](../images/avset.PNG)

- Azure **_Virtual Machine Scale Sets_** let you create and manage a group of identical, load balanced VMs.
- Azure Batch enables large-scale job scheduling and compute management with the ability to scale to tens, hundreds, or thousands of VMs.

### Containers

A container is a modified runtime environment built on top of a host OS that executes your application. A container doesn't use virtualization, so it doesn't waste resources simulating virtual hardware with a redundant OS. This environment typically makes containers more lightweight than VMs.

- Azure Supports Docker Containers
- Several Ways to manager containers in Azure
  - Azure Container Instances (ACI)
    - Azure Container Instances (ACI) offers the fastest and simplest way to run a container in Azure. You don't have to manage any virtual machines or configure any additional services. It is a PaaS offering that allows you to upload your containers and execute them directly with automatic elastic scale.
  - Azure Kubernetes Service (AKS)
    - The task of automating, managing, and interacting with a large number of containers is known as orchestration. Azure Kubernetes Service (AKS) is a complete orchestration service for containers with distributed architectures with multiple containers.

#### MicroService Architecture

- Containers are often used to create solutions using a microservice architecture. This architecture is where you break solutions into smaller, independent pieces. For example, you may split a website into a container hosting your front end, another hosting your back end, and a third for storage. This split allows you to separate portions of your app into logical sections that can be maintained, scaled, or updated independently.

### App Service

- This platform as a service (PaaS) allows you to focus on the website and API logic while Azure handles the infrastructure to run and scale your web applications.
- You pay for the Azure compute resources your app uses while it processes requests based on the App Service Plan you choose.
- With Azure App Service, you can host most common web app styles including:
  - Web Apps
  - API Apps
  - WebJobs
    - WebJobs allows you to run a program (.exe, Java, PHP, Python, or Node.js) or script (.cmd, .bat, PowerShell, or Bash) in the same context as a web app, API app, or mobile app. They can be scheduled, or run by a trigger. WebJobs are often used to run background tasks as part of your application logic
  - Mobile Apps

### Serverless Computing

- Serverless computing encompasses three ideas:
  - the abstraction of servers
  - an event-driven scale
  - and micro-billing:
- Two implementations of Serverless
  - Azure Functions
    - Azure Functions scale automatically based on demand, so they're a solid choice when demand is variable.
    - Using a VM-based approach, you'd incur costs even when the VM is idle. With functions, Azure runs your code when it's triggered and automatically deallocates resources when the function is finished.
    - Azure Functions can be either stateless (the default), where they behave as if they're restarted every time they respond to an event, or stateful (called "Durable Functions"), where a context is passed through the function to track prior activity.
  - Azure Logic Apps - Where Functions execute code, Logic Apps execute workflows designed to automate business scenarios and built from predefined logic blocks.

**NOTE**: Azure Functions are best suited for variable high demands, as containers and VMs are not as responsive as Functions. Functions are event-based and can scale instantly to process spikes in traffic. They are cost effective as well.

---

## Module 6

### Type of Data

- Structured or Relational: Usually with a schema and in tabular form
- Semi-Structured: Key Value pairs, or NoSQL
- UnStructured: Blobs

### Various Storage Options in Azure

- Azure SQL: relational Db as a service (DaaS)
- Azure Cosmos DB: supports schema less data
- Azure Blob Storage
- Azure Data Lake Storage: The Data Lake feature allows you to perform analytics on your data usage and prepare reports. Data Lake is a large repository that stores both structured and unstructured data.
- Azure Files: Azure Files offers fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol.
- Azure Queue
  - Create a backlog of work and to pass messages between different Azure web servers.
  - Distribute load among different web servers/infrastructure and to manage bursts of traffic.
  - Build resilience against component failure when multiple users access your data at the same time.
- Disk Storage: Disk storage provides disks for virtual machines, applications, and other services to access and use as they need, similar to how they would in on-premises scenarios.

#### Storage Tiers

1. Hot Storage Tiers: for data that is accessed frequently.
2. Cool Storage tier: infrequently accessed and stored for at least 30 days
3. Archive storage tier: rarely accessed & stored for 180 days

#### Encryption for storage services

1. Azure Storage Service Encryption (SSE) for data at rest. It encrypts the data before storing it and decrypts the data before retrieving it
2. Client side encryption: is where the data is already encrypted by the client libraries. Azure stores the data in the encrypted state at rest, which is then decrypted during retrieval.

### On premises vs Azure Storage

- Cost Effectiveness: Azure data storage provides a pay-as-you-go pricing model, which is often appealing to businesses as an operating expense instead of an upfront capital cost.
- Reliability: Azure data storage provides data backup, load balancing, disaster recovery, and data replication
- Agility

---

## Module 7

### Networking options in Azure

#### N-tier architecture

An N-tier architecture divides an application into two or more logical tiers. Architecturally, a higher tier can access services from a lower tier, but a lower tier should never access a higher tier.

Tiers help separate concerns and are ideally designed to be reusable. Using a tiered architecture also simplifies maintenance. Tiers can be updated or replaced independently, and new tiers can be inserted if needed.

The following is a 3-tier application, running on 3 VMs

![3-tier](../images/3tier.PNG)

In the above diagram:

- Azure Region: East US above
  - A Region is one or more Azure data centers within a specific geographical location.
- Virtual Network:
  - A virtual network is a logically isolated network on Azure
  - A virtual network is scoped to a single region; however, multiple virtual networks from different regions can be connected together using virtual network peering.
  - Virtual networks can be segmented into one or more **subnets**. Subnets help you organize and secure your resources in discrete sections. The web, application, and data tiers each have a single VM. All three VMs are in the same virtual network but are in separate subnets.
  - Users interact with the web tier directly, so that VM has a public IP address along with a private IP address. Users don't interact with the application or data tiers, so these VMs each have a private IP address only.
  - You can also keep your service or data tiers in your on-premises network, placing your web tier into the cloud, but keeping tight control over other aspects of your application. A **VPN gateway (or virtual network gateway)**, enables this scenario.
  - Azure manages the physical hardware for you. You configure virtual networks and gateways through software, which enables you to treat a virtual network just like your own network.
- Network Security Group
  - A network security group, or NSG, allows or denies inbound network traffic to your Azure resources. Think of a network security group as a cloud-level firewall for your network.
  - For example, notice that the VM in the web tier allows inbound traffic on ports 22 (SSH) and 80 (HTTP). This VM's network security group allows inbound traffic over these ports from all sources. You can configure a network security group to accept traffic only from known sources, such as IP addresses that you trust.

### Availability and Load Balancers

#### Availability

- Availability refers to how long your service is up and running without interruption. High availability, or highly available, refers to a service that's up and running for a long period of time.
- Five nines availability means that the service is guaranteed to be running 99.999 percent of the time.

#### Resiliency

- Resiliency refers to a system's ability to stay operational during abnormal conditions.
- e.g
  - Natural Disasters
  - Planned or Unplanned System Maintenance
  - Spikes in traffic
  - threats and attacks (e.g DDoS)

#### Load Balancer

- A load balancer distributes traffic evenly among each system in a pool. A load balancer can help you achieve both high availability and resiliency.
- If because of load, we start to add VMs, the problem is that each of those VMs have their own IP Address.
- The answer is to use a load balancer to distribute traffic. The load balancer becomes the entry point to the user. The user doesn't know (or need to know) which system the load balancer chooses to receive the request.

Azure Load Balancer distributes traffic among similar systems, making your services more highly available.

If one system is unavailable, Azure Load Balancer stops sending traffic to it. It then directs traffic to one of the responsive servers.

#### Azure Load Balancer

- Azure Load Balancer is a load balancer service that Microsoft provides that helps take care of the maintenance for you.
- Load Balancer supports inbound and outbound scenarios, provides low latency and high throughput, and scales up to millions of flows for all Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) applications.
- You can use Load Balancer with incoming internet traffic, internal traffic across Azure services, port forwarding for specific traffic, or outbound connectivity for VMs in your virtual network.

#### Azure Application Gateway

- If all your traffic is HTTP, a potentially better option is to use Azure Application Gateway. Application Gateway is a load balancer designed for web applications. It uses Azure Load Balancer at the transport level (TCP) and applies sophisticated URL-based routing rules to support several advanced scenarios.
- This type of routing is known as application layer (OSI layer 7) load balancing since it understands the structure of the HTTP message.
- Benefits
  - **Cookie affinity**. Useful when you want to keep a user session on the same backend server.
  - **SSL termination**. Application Gateway can manage your SSL certificates and pass unencrypted traffic to the backend servers to avoid encryption/decryption overhead. It also supports full end-to-end encryption for applications that require that.
  - **Web application firewall**. Application gateway supports a sophisticated firewall (WAF) with detailed monitoring and logging to detect malicious attacks against your network infrastructure.
  - **URL rule-based routes**. Application Gateway allows you to route traffic based on URL patterns, source IP address and port to destination IP address and port. This is helpful when setting up a content delivery network.
  - **Rewrite HTTP headers**. You can add or remove information from the inbound and outbound HTTP headers of each request to enable important security scenarios, or scrub sensitive information such as server names.

#### Content Delivery Network (CDN)

- A content delivery network (CDN) is a distributed network of servers that can efficiently deliver web content to users.
- It is a way to get content to users in their local region to minimize latency.
- CDN can be hosted in Azure or any other location. You can cache content at strategically placed physical nodes across the world and provide better performance to end users.
- Typical usage scenarios include web applications containing multimedia content, a product launch event in a particular region, or any event where you expect a high-bandwidth requirement in a region.

#### DNS

- You can bring your own DNS server or use Azure DNS, a hosting service for DNS domains that runs on Azure infrastructure.

### Network Latency

Latency refers to the time it takes for data to travel over the network. Latency is typically measured in milliseconds.

Compare latency to bandwidth. Bandwidth refers to the amount of data that can fit on the connection. Latency refers to the time it takes for that data to reach its destination.

Factors such as the type of connection you use and how your application is designed can affect latency. But perhaps the biggest factor is distance.

#### Azure Traffic Manager

Azure Traffic Manager. Traffic Manager uses the DNS server that's closest to the user to direct user traffic to a globally distributed endpoint.

Traffic Manager doesn't see the traffic that's passed between the client and server. Rather, it directs the client web browser to a preferred endpoint. Traffic Manager can route traffic in a few different ways, such as to the endpoint with the lowest latency.

#### Load Balancer vs Traffic Manager

- Azure Load Balancer distributes traffic within the same region to make your services more highly available and resilient. Traffic Manager works at the DNS level, and directs the client to a preferred endpoint. This endpoint can be to the region that's closest to your user.

- Load Balancer and Traffic Manager both help make your services more resilient, but in slightly different ways. When Load Balancer detects an unresponsive VM, it directs traffic to other VMs in the pool. Traffic Manager monitors the health of your endpoints. When Traffic Manager finds an unresponsive endpoint, it directs traffic to the next closest endpoint that is responsive.

---

## Module 8

### Defense in Depth

Defense in depth is a strategy that employs a series of mechanisms to slow the advance of an attack aimed at acquiring unauthorized access to information. Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure. Microsoft applies a layered approach to security, both in physical data centers and across Azure services. The objective of defense in depth is to protect and prevent information from being stolen by individuals who are not authorized to access it.

### Security is a shared responsibility

The responsibility of the user increases as we move from SaaS to PaaS to IaaS.

### Azure Security Center

Security Center is a monitoring service that provides threat protection across all of your services both in Azure, and on-premises.

You can reduce the chances of a significant security event by configuring a security policy, and then implementing the recommendations provided by Azure Security Center.

A security policy defines the set of controls that are recommended for resources within that specified subscription or resource group. In Security Center, you define policies according to your company's security requirements.

Security Center analyzes the security state of your Azure resources. When Security Center identifies potential security vulnerabilities, it creates recommendations based on the controls set in the security policy.

### Azure Active Directory

- Cloud based Id service
- provides services such as
  - Authentication
  - Single-Sign-On (SSO)
    - SSO enables users to remember only one ID and one password to access multiple applications. A single identity is tied to a user, simplifying the security model. As users change roles or leave an organization, access modifications are tied to that identity, greatly reducing the effort needed to change or disable accounts.
  - Application Mgmt
  - B2B Id Services
  - B2C
  - Device Management

#### Multi Factor Auth

is done based on 3 things

- something you know
- something you possess
- something you are

#### Providing Identities to services

##### Identity

- An identity is just a thing that can be authenticated. Obviously, this includes users with a user name and password, but it can also include applications or other servers, which might authenticate with secret keys or certificates.

##### Principal

- A principal is an identity acting with certain roles or claims.

- Usually, it is not useful to consider identity and principal separately, but think of using 'sudo' on a Bash prompt in Linux or on Windows using "run as Administrator.".

- In both those cases, you are still logged in as the same identity as before, but you've changed the role under which you are executing.

- Groups are often also considered principals because they can have rights assigned.

##### Service Principal

- A service principal is an identity that is used by a service or application. And like other identities, it can be assigned roles.

##### Managed identities for Azure services

The creation of service principals can be a tedious process, and there are a lot of touch points that can make maintaining them difficult. Managed identities for Azure services are much easier and will do most of the work for you.

A managed identity can be instantly created for any Azure service that supports it—and the list is constantly growing. When you create a managed identity for a service, you are creating an account on your organization's Active Directory (a specific organization's Active Directory instance is known as an **"Active Directory Tenant"**). The Azure infrastructure will automatically take care of authenticating the service and managing the account. You can then use that account like any other Azure AD account, including allowing the authenticated service secure access of other Azure resources.

#### Role Based Access Control

- Roles are sets of permissions, like "Read-only" or "Contributor", that users can be granted to access an Azure service instance.

- Identities are mapped to roles directly or through group membership. Separating security principals, access permissions, and resources provides simple access management and fine-grained control. Administrators are able to ensure the minimum necessary permissions are granted.

- Roles can be granted at the individual service instance level, but they also flow down the Azure Resource Manager hierarchy.

- Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances.

#### Privileged Identity Management

In addition to managing Azure resource access with role-based access control (RBAC), a comprehensive approach to infrastructure protection should consider including the ongoing auditing of role members as their organization changes and evolves.

Azure AD Privileged Identity Management (PIM) is an additional, paid-for offering that provides oversight of role assignments, self-service, and just-in-time role activation and Azure AD and Azure resource access reviews.

### Encryption

Encryption is the process of making data unreadable and unusable to unauthorized viewers.

#### Symmetric encryption

uses the same key to encrypt and decrypt the data.

#### Asymmetric encryption

uses a public key and private key pair. Either key can encrypt but a single key can't decrypt its own encrypted data. To decrypt, you need the paired key. Asymmetric encryption is used for things like Transport Layer Security (TLS) (used in HTTPS) and data signing.

Encryption is typically approached in two ways:

- Encryption at rest
  - Data at rest is the data that has been stored on a physical medium. This data could be stored on the disk of a server, data stored in a database, or data stored in a storage account.
  - Regardless of the storage mechanism, encryption of data at rest ensures that the stored data is unreadable without the keys and secrets needed to decrypt it.
  - If an attacker was to obtain a hard drive with encrypted data and did not have access to the encryption keys, the attacker would not compromise the data without great difficulty.
- Encryption in transit
  - Data in transit is the data actively moving from one location to another, such as across the internet or through a private network.
  - Secure transfer can be handled by several different layers. It could be done by encrypting the data at the application layer prior to sending it over a network. HTTPS is an example of application layer in transit encryption.

#### Encryption in Azure

- Encrypt raw storage: **Azure Storage Service Encryption** for data at rest in Azure Managed Disks, Azure Blob storage, Azure Files, or Azure Queue storage
- Encrypt virtual machine disks: **Azure Disk Encryption** is a capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.
  - Azure Disk Encryption leverages the industry-standard BitLocker feature of Windows and the dm-crypt feature of Linux to provide volume encryption for the OS and data disks. The solution is integrated with Azure Key Vault
- Encrypt databases: **Transparent data encryption (TDE)** helps protect Azure SQL Database and Azure Data Warehouse
  - TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key. By default, Azure provides a unique encryption key per logical SQL Server instance and handles all the details. Bring your own key (BYOK) is also supported with keys stored in Azure Key Vault
- Encrypt secrets
  - **Azure Key Vault** is a centralized cloud service for storing your application secrets.
  - Secrets Mgmt
  - Key Management
  - Certificate Management: like SSL/TLS (Secure Sockets Layer)

### Azure Certificates

- Transport Layer Security (TLS) is the basis for encryption of website data in transit. TLS uses certificates to encrypt and decrypt data. However, these certificates have a lifecycle that requires administrator management. A common security problem with websites is having expired TLS certificates that open security vulnerabilities.

- Certificates used in Azure are x.509 v3 and can be signed by a trusted certificate authority, or they can be self-signed. A self-signed certificate is signed by its own creator; therefore, it is not trusted by default. Most browsers can ignore this problem. However, you should only use self-signed certificates when developing and testing your cloud services. These certificates can contain a private or a public key and have a thumbprint that provides a means to identify a certificate in an unambiguous way. This thumbprint is used in the Azure configuration file to identify which certificate a cloud service should use.

#### Types of Certificates

##### Service certificates

Service certificates are attached to cloud services and enable secure communication to and from the service. For example, if you deploy a web site, you would want to supply a certificate that can authenticate an exposed HTTPS endpoint.

You can upload service certificates to Azure either using the Azure portal or by using the classic deployment model. Service certificates are associated with a specific cloud service. They are assigned to a deployment in the service definition file.

##### Management certificates

Management certificates allow you to authenticate with the classic deployment model. Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services. However, these types of certificates are not related to cloud services.

### Network Protection

#### Firewall

Firewall is a service that grants server access based on the originating IP address of each request.

Multiple options for inbound protection in Azure

##### Azure Firewall

It is a fully stateful firewall as a service with built-in high availability and unrestricted cloud scalability. Azure Firewall provides inbound protection for non-HTTP/S protocols. Examples of non-HTTP/S protocols include: Remote Desktop Protocol (RDP), Secure Shell (SSH), and File Transfer Protocol (FTP). It also provides outbound, network-level protection for all ports and protocols, and application-level protection for outbound HTTP/S.

##### Azure Application Gateway

is a load balancer that includes a Web Application Firewall (WAF) that provides protection from common, known vulnerabilities in websites. It is designed to protect HTTP traffic.

##### Network Virtual appliances (NVAs)

are ideal options for non-HTTP services or advanced configurations, and are similar to hardware firewall appliances.

#### Azure DDoS Protection

#### Virtual Network Security

Once inside a virtual network (VNet), it's crucial that you limit communication between resources to only what is required.

For communication between virtual machines, Network Security Groups (NSGs) are a critical piece to restrict unnecessary communication.

You can completely remove public internet access to your services by restricting access to service endpoints. With service endpoints, Azure service access can be limited to your virtual network.

Virtual private network (VPN) connections are a common way of establishing secure communication channels between networks. Connections between Azure Virtual Network and an on-premises VPN device are a great way to provide secure communication between your network and your VNet on Azure.

To provide a dedicated, private connection between your network and Azure, you can use Azure ExpressRoute. ExpressRoute lets you extend your on-premises networks into the Microsoft cloud over a private connection facilitated by a connectivity provider

#### Microsoft Azure Information Protection

(sometimes referred to as AIP) is a cloud-based solution that helps organizations classify and optionally protect documents and emails by applying labels.

#### Azure Advanced Threat Protection (Azure ATP)

is a cloud-based security solution that identifies, detects, and helps you investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.

#### The Microsoft Security Development Lifecycle (SDL)

introduces security and privacy considerations throughout all phases of the development process. It helps developers build highly secure software, address security compliance requirements, and reduce development costs.

---

## Module 9

### Azure Policy

Azure Policy is an Azure service you use to create, assign and, manage policies. These policies enforce different rules and effects over your resources so that those resources stay compliant with your corporate standards and service level agreements.

Azure Policy meets this need by evaluating your resources for noncompliance with assigned policies. For example, you might have a policy that allows virtual machines of only a certain size in your environment. After this policy is implemented, new and existing resources are evaluated for compliance. With the right type of policy, existing resources can be brought into compliance.

#### How are Azure Policy and RBAC different?

At first glance, it might seem like Azure Policy is a way to restrict access to specific resource types similar to role-based access control (RBAC). However, they solve different problems. RBAC focuses on user actions at different scopes. You might be added to the contributor role for a resource group, allowing you to make changes to anything in that resource group. Azure Policy focuses on resource properties during deployment and for already-existing resources. Azure Policy controls properties such as the types or locations of resources. Unlike RBAC, Azure Policy is a default-allow-and-explicit-deny system.

To apply a policy, you will:

- Create a policy definition
- Assign a definition to a scope of resources
- View policy evaluation results

## What is a policy definition?

A policy definition expresses what to evaluate and what action to take. For example, you could ensure all public websites are secured with HTTPS, prevent a particular storage type from being created, or force a specific version of SQL Server to be used.

Examples

- Allowed Storage Account SKUs
- Allowed Resource Type
- Allowed Locations
- Allowed Virtual Machine SKUs
- Not allowed resource types

The policy definition itself is represented as a JSON file - you can use one of the pre-defined definitions in the portal or create your own (either modifying an existing one or starting from scratch).

Once you've defined one or more policy definitions, you'll need to assign them. A policy assignment is a policy definition that has been assigned to take place within a specific scope.

This scope could range from a full subscription down to a resource group. Policy assignments are inherited by all child resources. This inheritance means that if a policy is applied to a resource group, it is applied to all the resources within that resource group. However, you can exclude a subscope from the policy assignment. For example, we could enforce a policy for an entire subscription and then exclude a few select resource groups.

#### Policy effects

Requests to create or update a resource through Azure Resource Manager are evaluated by Azure Policy first.

Azure Policy can allow a resource to be created even if it doesn't pass validation. In these cases, you can have it trigger an audit event that can be viewed in the Azure Policy portal, or through command-line tools.

### Initiatives

Managing a few policy definitions is easy, but once you have more than a few, you will want to organize them. That's where initiatives come in.

An initiative definition is a set or group of policy definitions to help track your compliance state for a larger goal.

### Enterprise governance management

Access management occurs at the Azure subscription level. This control allows an organization to configure each division of the company in a specific fashion based on their responsibilities and requirements. Planning and keeping rules consistent across subscriptions can be challenging without a little help.

#### Azure Management Group

Azure Management Groups are containers for managing access, policies, and compliance across multiple Azure subscriptions.

### Azure BluePrints

Adhering to security or compliance requirements, whether government or industry requirements, can be difficult and time-consuming. To help you with auditing, traceability, and compliance of your deployments, use Azure Blueprint artifacts and tools.

Just as a blueprint allows an engineer or an architect to sketch a project's design parameters, Azure Blueprints enables cloud architects and central information technology groups to define a repeatable set of Azure resources that implements and adheres to an organization's standards, patterns, and requirements.

Azure Blueprints makes it possible for development teams to rapidly build and deploy new environments with the trust they're building within organizational compliance using a set of built-in components, such as networking, to speed up development and delivery.

Azure Blueprints is a declarative way to orchestrate the deployment of various resource templates and other artifacts, such as:

- Role assignments
- Policy assignments
- Azure Resource Manager templates
- Resource groups

The Azure Blueprints service is backed by the globally distributed Azure Cosmos database. Blueprint objects are replicated to multiple Azure regions. This replication provides low latency, high availability, and consistent access to your blueprint objects, regardless of which region Blueprints deploys your resources to.

#### How is it different from Resource Manager templates?

The Azure Blueprints service is designed to help with environment setup. This setup often consists of a set of resource groups, policies, role assignments, and Resource Manager template deployments. A blueprint is a package to bring each of these artifact types together and allow you to compose and version that package—including through a CI/CD pipeline. Ultimately, each setup is assigned to a subscription in a single operation that can be audited and tracked.

Nearly everything that you want to include for deployment in Blueprints can be accomplished with a Resource Manager template. However, a Resource Manager template is a document that doesn't exist natively in Azure. Resource Manager templates are stored either locally or in source control. The template gets used for deployments of one or more Azure resources, but once those resources deploy there's no active connection or relationship to the template.

With Blueprints, the relationship between the blueprint definition (what should be deployed) and the blueprint assignment (what was deployed) is preserved. This connection supports improved tracking and auditing of deployments. Blueprints can also upgrade several subscriptions at once that are governed by the same blueprint.

#### How it's different from Azure Policy

A blueprint is a package or container for composing focus-specific sets of standards, patterns, and requirements related to the implementation of Azure cloud services, security, and design that can be reused to maintain consistency and compliance.

A policy is a default-allow and explicit-deny system focused on resource properties during deployment and for already existing resources. It supports cloud governance by validating that resources within a subscription adhere to requirements and standards.

Including a policy in a blueprint enables the creation of the right pattern or design during assignment of the blueprint. The policy inclusion makes sure that only approved or expected changes can be made to the environment to protect ongoing compliance to the intent of the blueprint.

A policy can be included as one of many artifacts in a blueprint definition. Blueprints also support using parameters with policies and initiatives.

### Compliance Manager

Governing your own resources and how they are used is only part of the solution when using a cloud provider. You also have to understand how the provider manages the underlying resources you are building on.

Microsoft takes this management seriously and provides full transparency with four sources:

- Microsoft Privacy Statement
- Microsoft Trust Center
- Service Trust Portal
- Compliance Manager

The Microsoft privacy statement explains what personal data Microsoft processes, how Microsoft processes it, and for what purposes.

Trust Center is a website resource containing information and details about how Microsoft implements and supports security, privacy, compliance, and transparency in all Microsoft cloud products and services

The Service Trust Portal (STP) hosts the Compliance Manager service, and is the Microsoft public site for publishing audit reports and other compliance-related information relevant to Microsoft's cloud services.

Compliance Manager is a workflow-based risk assessment dashboard within the Service Trust Portal that enables you to track, assign, and verify your organization's regulatory compliance activities related to Microsoft professional services and Microsoft cloud services such as Office 365, Dynamics 365, and Azure.

### Monitor Service Health

Defining policy and access provides fine-grained control over resources in your cloud IT infrastructure. Once those resources are deployed, you will want to know about any issues or performance problems they might encounter.

Azure provides two primary services to monitor the health of your apps and resources.

- Azure Monitor
- Azure Service Health

#### Azure Monitor

Azure Monitor maximizes the availability and performance of your applications by delivering a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. It helps you understand how your applications are performing and proactively identifies issues affecting them and the resources they depend on.

#### Azure Service Health

is a suite of experiences that provide personalized guidance and support when issues with Azure services affect you. It can notify you, help you understand the impact of issues, and keep you updated as the issue is resolved. Azure Service Health can also help you prepare for planned maintenance and changes that could affect the availability of your resources.

### Questions

You can download published audit reports and other compliance-related information related to Microsoft’s cloud service from the Service Trust Portal (True)

Which Azure service allows you to configure fine-grained access management for Azure resources, enabling you to grant users only the rights they need to perform their jobs? (Role Based Access Control)

Which Azure service allows you to create, assign, and, manage policies to enforce different rules and effects over your resources and stay compliant with your corporate standards and service-level agreements (SLAs)? (Azure Policy)

Which of the following services provides up-to-date status information about the health of Azure services? (Azure Service Health)

Where can you obtain details about the personal data Microsoft processes, how Microsoft processes it, and for what purposes?

Trust Center provides you with information about security, compliance, privacy, and transparency options available from Microsoft.

---

## Module 10

### Usage Meters

When you provision an Azure resource, Azure creates one or more meter instances for that resource. The meters track the resources' usage, and generate a usage record that is used to calculate your bill.

For example, a single virtual machine that you provision in Azure might have the following meters tracking its usage:

- Compute Hours
- IP Address Hours
- Data Transfer In
- Data Transfer Out
- Standard Managed Disk
- Standard Managed Disk Operations
- Standard IO-Disk
- Standard IO-Block Blob Read
- Standard IO-Block Blob Write
- Standard IO-Block Blob Delete

### Factors affecting costs

#### Resource type

Costs are resource-specific, so the usage that a meter tracks and the number of meters associated with a resource depend on the resource type.

Each meter tracks a particular kind of usage. For example, a meter might track bandwidth usage (ingress or egress network traffic in bits-per-second), the number of operations, size (storage capacity in bytes), or similar items.

The usage that a meter tracks correlates to a number of billable units. The rate per billable unit depends on the resource type you are using. Those units are charged to your account for each billing period.

#### Services

Azure usage rates and billing periods can differ between Enterprise, Web Direct, and Cloud Solution Provider (CSP) customers. Some subscription types also include usage allowances, which affect costs.

#### Location

Azure has datacenters all over the world. Usage costs vary between locations that offer particular Azure products, services, and resources based on popularity, demand, and local infrastructure costs.

#### Azure Billing Zones

Bandwidth refers to data moving in and out of Azure datacenters. Most of the time inbound data transfers (data going into Azure datacenters) are free. For outbound data transfers (data going out of Azure datacenters), the data transfer pricing is based on Billing Zones.

A Zone is a geographical grouping of Azure Regions for billing purposes. The following zones exist and include the listed countries (regions).

- Zone 1: United States, US Government, Europe, Canada, UK, France, Switzerland
- Zone 2: East Asia, Southeast Asia, Japan, Australia, India, Korea
- Zone 3: Brazil, South Africa, UAE
- DE Zone 1: Germany

In most zones, the first outbound 5 gigabytes (GB) per month are free. After that amount, you are billed a fixed price per GB.

**Note:** Billing zones aren't the same as an Availability Zone. In Azure, the term zone is for billing purposes only, and the full term Availability Zone refers to the failure protection that Azure provides for datacenters.

**Note:** PaaS is typically less expensive than IaaS
