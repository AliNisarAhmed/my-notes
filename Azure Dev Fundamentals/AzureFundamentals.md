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
