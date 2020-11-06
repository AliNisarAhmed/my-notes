## Configure Logging in a web app

Command

`az webapp log configure`

```
az webapp log config [--application-logging {azureblobstorage, filesystem, off}]
                     [--detailed-error-messages {false, true}]
                     [--docker-container-logging {filesystem, off}]
                     [--failed-request-tracing {false, true}]
                     [--ids]
                     [--level {error, information, verbose, warning}]
                     [--name]
                     [--resource-group]
                     [--slot]
                     [--subscription]
                     [--web-server-logging {filesystem, off}]
```

---

### Kubernetes Ingress Controller

An ingress controller is a piece of software that provides **reverse proxy**, **configurable traffic routing**, and **TLS termination** for **Kubernetes services**. Kubernetes ingress resources are used to configure the ingress rules and routes for individual Kubernetes services. Using an ingress controller and ingress rules, a single IP address can be used to route traffic to multiple services in a Kubernetes cluster.

[more here](https://docs.microsoft.com/en-us/azure/aks/ingress-basic)

---

To secure a logic app, we use **Integration Service Environment**

---

### Azure Service Bus - Topic Filters

- **Boolean Filters**: The **TrueFilter** and **FalseFilter** either cause all arriving messages (true) or none of the arriving messages (false) to be selected for the subscription.
- **SQL Filters** - A SqlFilter holds a SQL-like conditional expression that is evaluated in the broker against the arriving messages' user-defined properties and system properties. All system properties must be prefixed with sys. in the conditional expression. The SQL-language subset for filter conditions tests for the existence of properties (EXISTS), null-values (IS NULL), logical NOT/AND/OR, relational operators, simple numeric arithmetic, and simple text pattern matching with LIKE.
- **Correlation Filters** - A CorrelationFilter holds a set of conditions that are matched against one or more of an arriving message's user and system properties. A common use is to match against the CorrelationId property, but the application can also choose to match against the following properties:

  - ContentType
  - Label
  - MessageId
  - ReplyTo
  - ReplyToSessionId
  - SessionId
  - To
  - any user-defined properties.

A match exists when an arriving message's value for a property is equal to the value specified in the correlation filter. For string expressions, the comparison is case-sensitive. When specifying multiple match properties, the filter combines them as a logical AND condition, meaning for the filter to match, all conditions must match.

_All filters evaluate message properties. Filters can't evaluate the message body._

---

Shared web app service plan does not support Auto-Scaling.

At least Standard App Service plan is required to enable auto-scaling

---

### Application Insights Profiler for web apps

To enable Application Insights Profiler for a web app which captures the data automatically at scale without negatively affecting to end users, we need to enab;le _Always On_ Settings

If a web app sits idle for too long, system unloads the website, and when traffic returns, the system needs to load the web app which causes longer response time and higher utilization of resources.

---

A .Net Core app is used to work with blobs in azure storage account. Authentication is Azure AD creds.

RBAC access has been implemented on the containers. These roles have been assigned to the users.

We have to configure the app so that user's permission can be used with the Azure blob containers.

- MS Graph API would need `User.Read` permission, which is a Delegated permission, to read user role.
- Azure Storage API would need `user_impersonation` permission, a Delegated permission. (this permission is given to the app to call azure storage api on behalf of the user.)

[Ref](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth-aad-app?tabs=dotnet)

---

When there are no distinct values that are suitable to be used as partition keys for Azure Cosmos DB, then there are two options

1. Employing a strategy of concatenation of multiple property values with a random suffix appended.
2. Using a hash-suffix that is appended to a property value.

It's the best practice to have a partition key with many distinct values, such as hundreds or thousands. The goal is to distribute your data and workload evenly across the items associated with these partition key values. If such a property doesn’t exist in your data, you can construct a synthetic partition key. This document describes several basic techniques for generating a synthetic partition key for your Cosmos container.

[Ref](https://docs.microsoft.com/en-us/azure/cosmos-db/synthetic-partition-keys)

---

When we return a value from an Azure function, the output binding must use `$return` output parameter.

---

## Isolated Service Plan (Web app service plan)

When there is a requirement for resources to be located in an isolated network, we need to use the isolated pricing tier.

The isolated service plan is designed to run mission critical workloads, that are required to run in a virtual network.

The provate environment used with an Isolated plan is called `App Service Environment`. The plan can scale to 100 instances with more available upon request.

---

To run webjobs for 3 customers, which need to run in isolation, we can use 3 VMs, one for each customer.

---

## Azure web app service deployment from github

The github documentation for kudu-based deployments mentions

- `.deployment` files let you override the default deployment heuristics.
- We can call custom deployment scripts (like calling a script that generates the static content)

---

## Azure DB Migration Service

is a fully managed service desinged to enable seamless migrations from multiple db sources to azure data platforms with minimal downtime (online migrations)

---

## Using Service Principal to login to Azure Container Registry

[Ref](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentications)

- Each Container Registry includes an admin user account, which is disabled by default. This account is designed for a single user to access the registry
- Using service Principals with each regisry, we can define different access for different applications. This allows headless authentication. Also allows RBAC access
- We can also use the Azure CLI Command `az acr login --name <acrname>`

---

## Creating Azure Cosmos DB first and then creating a Table

[Ref](https://docs.microsoft.com/en-us/azure/cosmos-db/scripts/cli/table/create?toc=/cli/azure/toc.json)
