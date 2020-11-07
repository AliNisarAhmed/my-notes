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

---

# Practice Test 2

---

## How CDN Works

- A user (Alice) requests a file (also called an asset) by using a URL with a special domain name, such as <endpoint name>.azureedge.net. This name can be an endpoint hostname or a custom domain. The DNS routes the request to the best performing POP location (Point of Presence, a collection of edge servers in a location), which is usually the POP that is geographically closest to the user.

- If no edge servers in the POP have the file in their cache, the POP requests the file from the origin server. The origin server can be an Azure Web App, Azure Cloud Service, Azure Storage account, or any publicly accessible web server.

- The origin server returns the file to an edge server in the POP.

- An edge server in the POP caches the file and returns the file to the original requestor (Alice). The file remains cached on the edge server in the POP until the time-to-live (TTL) specified by its HTTP headers expires. If the origin server didn't specify a TTL, the default TTL is seven days.

- Additional users can then request the same file by using the same URL that Alice used, and can also be directed to the same POP.

- If the TTL for the file hasn't expired, the POP edge server returns the file directly from the cache. This process results in a faster, more responsive user experience.

---

## Azure Redis best Practice - Memory Management

There are several things related to memory usage within your Redis server instance that you may want to consider. Here are a few:

- Choose an **eviction policy** that works for your application. The default policy for Azure Redis is volatile-lru, which means that only keys that have a TTL value set will be eligible for eviction. If no keys have a TTL value, then the system won't evict any keys. If you want the system to allow any key to be evicted if under memory pressure, then you may want to consider the allkeys-lru policy.

- Set an **expiration value on your keys**. An expiration will remove keys proactively instead of waiting until there's memory pressure. When eviction does kick in because of memory pressure, it can cause additional load on your server.

[Ref](https://docs.microsoft.com/en-us/azure/azure-cache-for-redis/cache-best-practices)

---

## Azure Monitor - user behaviour analytics

To track users over time, Application Insights requires a way to identify them. The Events tool is the only Usage tool that does not require a user ID or a session ID.

- **Funnels**: If your application involves multiple stages, you need to know if most customers are progressing through the entire process, or if they are ending the process at some point. The progression through a series of steps in a web application is known as a funnel.
- **Users, Sessions and Events segmentation tool**
  - _User Tool_: How many people used your app and its features. Users are counted by using anonymous IDs stored in browser cookies. A single person using different browsers or machines will be counted as more than one user.
  - _Sessions tool_: How many sessions of user activity have included certain pages and features of your app. A session is counted after half an hour of user inactivity, or after 24 hours of continuous use.
  - _Events tool_: How often certain pages and features of your app are used. A page view is counted when a browser loads a page from your app, provided you have instrumented it.
- **Cohorts**: A cohort is a set of users, sessions, events, or operations that have something in common. In Azure Application Insights, cohorts are defined by an analytics query. In cases where you have to analyze a specific set of users or events repeatedly, cohorts can give you more flexibility to express exactly the set you’re interested in.
- **Impact** analyzes how load times and other properties influence conversion rates for various parts of your app. To put it more precisely, it discovers how any dimension of a page view, custom event, or request affects the usage of a different page view or custom event.
- **Retention**: The retention feature in Azure Application Insights helps you analyze how many users return to your app, and how often they perform particular tasks or achieve goals.
- **The User Flows** tool visualizes how users navigate between the pages and features of your site.

---

## How to set up Azure Service bus for Restaurant Delivery service

1. Create Namespace - i.e. application level container
2. Create Topic - Useful in pub/sub scenarios - a message is sent to all topics - a topic can have multiple subscriptions
3. Create One Subscription per Driver - Each user/driver who wants to receive updates when a particular Restaurant has an order will create a subscription. Subscribers can define which messages they want to receive from a topic. These messages are specified in the form of one or more named subscription rules

---

## Query Strings in Azure CDN

With Azure Content Delivery Network (CDN), you can control how files are cached for a web request that contains a query string.

Three query string modes are available:

- **Ignore query strings**: Default mode. In this mode, the CDN point-of-presence (POP) node passes the query strings from the requestor to the origin server on the first request and caches the asset. All subsequent requests for the asset that are served from the POP ignore the query strings until the cached asset expires.

- **Bypass caching for query strings**: In this mode, requests with query strings are not cached at the CDN POP node. The POP node retrieves the asset directly from the origin server and passes it to the requestor with each request.

-**Cache every unique URL**: In this mode, each request with a unique URL, including the query string, is treated as a unique asset with its own cache. For example, the response from the origin server for a request for example.ashx?q=test1 is cached at the POP node and returned for subsequent caches with the same query string. A request for example.ashx?q=test2 is cached as a separate asset with its own time-to-live setting.

---

- "Always On" Setting is available from Basic and above Azure App Service plan
