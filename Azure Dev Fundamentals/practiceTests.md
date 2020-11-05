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
