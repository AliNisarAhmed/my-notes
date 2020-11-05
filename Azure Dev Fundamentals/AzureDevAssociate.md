## 1 - Azure Functions

Business processes modeled in software are often called **workflows**. Azure includes four different technologies that you can use to build and implement workflows that integrate multiple systems:

The first two are **Design-First** Technologies, while the latter are **Code-first**.

- Logic Apps
  - for devs and IT Pros
  - GUI based
  - can also edit workflows in JSON
  - 200 connectors are available
  - A connector is a Logic Apps component that provides an interface to an external service.
  - For example, the Twitter connector
- Microsoft Power Automate

  - for office workers and business analysts
  - Build on top of logic app
  - fully GUI based
  - handles the following four different types of flows
    - Automated: A flow that is started by a trigger from some event. For example, the event could be the arrival of a new tweet or a new file being uploaded.
    - Button: Use a button flow to run a repetitive task with a single click from your mobile device.
    - Scheduled: A flow that executes on a regular basis such as once a week, on a specific date, or after 10 hours.
    - Business process: A flow that models a business process such as the stock ordering process or the complaints procedure.

- WebJobs

  - WebJobs are a part of the Azure App Service that you can use to run a program or script automatically. There are two kinds of WebJob:
  - Continuous. These WebJobs run in a continuous loop. For example, you could use a continuous WebJob to check a shared folder for a new photo.
  - Triggered. These WebJobs run when you manually start them or on a schedule.
  - You may choose WebJobs for the following reasons:
    - You want the code to be a part of an existing App Service application and to be managed as part of that application, for example in the same Azure DevOps environment.
    - You need close control over the object that listens for events that trigger the code. This object in question is the `JobHost` class, and you have more flexibility to modify its behavior in WebJobs.

- Azure Functions
  - The diff with WebJobs is that they do not provide close control of the `JobHost` class
  - `JobHost` class is the object that listens for events that trigger the code

**Notes**:

1. Microsoft Power Automate permit the creative team, who are not developers, to manage a process
2. WebJobs are the only technology that permits developers to control retry policies. So, they are useful in uncertain conditions where we want to ensure success.
3. Azure Logic Apps is the only one of the four technologies that provides a design-first approach intended for developers.
4. Webjobs can be made part of the existing app service, but...
5. Its not necessary to write a WebJob as part of the existing Azure App Service application, because you want to manage the procedure code separately.

### Azure Functions

- Azure Functions are Event triggered. The type of event that starts the function is called a **trigger**.

- **Note:** Every Azure Function must have exactly one trigger associated with it. If you want to use multiple triggers, you must create multiple functions.

#### Bindings

Bindings are a declarative way to connect data and services to your function. Bindings know how to talk to different services, which means you don't have to write code in your function to connect to data sources and manage connections.

#### Timer Trigger

- A timer trigger is a trigger that executes a function at a consistent interval. To create a timer trigger, you need to supply two pieces of information.

  - A Timestamp parameter name, which is simply an identifier to access the trigger in code.
  - A Schedule, which is a CRON expression that sets the interval for the timer.

##### CRON Expression

- A CRON expression is a string that consists of six fields that represent a set of times.

- The order of the six fields in Azure is: `{second}` `{minute}` `{hour}` `{day}` `{month}` `{day of the week}`.

- For example: A Trigger that executes every five minutes `"0 */5 * * * *"`

  - `*` Selects every value in a field. An asterisk `"*"` in the day of the week field means every day.
  - `,` Separates items in a list. A comma "1,3" in the day of the week field means just Mondays (day 1) and Wednesdays (day 3).
  - `-` Specifies a range. A hyphen "10-12" in the hour field means a range that includes the hours 10, 11, and 12.
  - `/` Specifies an increment A slash `"/10"` in the minutes field means an increment of every 10 minutes.

#### Http Trigger

- An HTTP trigger Authorization level is a flag that indicates if an incoming HTTP request needs an API key for authentication reasons.

- There are three Authorization levels:

  - Function
  - Anonymous
  - Admin

The Function and Admin levels are "key" based. To send an HTTP request, you must supply a key for authentication.

There are two types of keys: function and host. The difference between the two keys is their scope. Function keys are specific to a function.

Host keys apply to all functions inside the function app. If your Authorization level is set to Function, you can use either a function or a host key. If your Authorization level is set to Admin, you must supply a host key.

The Anonymous level means that there's no authentication required.

### Triggers vs Bindings

Triggers are what cause a function to run. A trigger defines how a function is invoked and a function must have exactly one trigger. Triggers have associated data, which is often provided as the payload of the function.

Binding to a function is a way of declaratively connecting another resource to the function; bindings may be connected as input bindings, output bindings, or both. Data from bindings is provided to the function as parameters.

You can mix and match different bindings to suit your needs. Bindings are optional and a function might have one or multiple input and/or output bindings.

Triggers and bindings let you avoid hardcoding access to other services. Your function receives data (for example, the content of a queue message) in function parameters. You send data (for example, to create a queue message) by using the return value of the function.

![Binding](binding.PNG)

#### What is a binding expression?

A binding expression is specialized text in function.json, function parameters, or code that is evaluated when the function is invoked to yield a value. For example, if you have a Service Bus Queue binding, you could use a binding expression to get the name of the queue from App Settings.
Types of binding expressions

- App settings
- Trigger file name
- Trigger metadata
- JSON payloads
- New GUID
- Current date and time

### Best Practices on protecting Azure Function

Protecting your endpoints using the authorization keys is not a recommended practice for production environments. You should only use authorization keys on testing or development environments for controlling the access to your API. For a production environment, you should use one of the following approaches:

- **Enable Function App Authorization/Authentication** This integrates your API with Azure Active Directory or other third-party identity providers to authenticate clients.
- **Use Azure API Management (APIM)** This secures the incoming request to your API, such as filtering by IP address or using authentication based on certificates.
- **Deploy your function in an App Service Environment (ASE)** - ASEs provides dedicated hosting environments that allow you to configure a single front-end gateway that can authenticate all incoming requests.

---

## Durable Azure Functions

Durable Functions is an extension of Azure Functions that enables you to perform long-lasting, stateful operations in Azure.

Some benefits of using Durable Functions include:

- They enable you to write event driven code. A durable function can wait asynchronously for one or more external events, and then perform a series of tasks in response to these events.
- You can chain functions together. You can implement common patterns such as fan-out/fan-in, which uses one function to invoke others in parallel, and then accumulate the results.
- You can orchestrate and coordinate functions, and specify the order in which functions should execute.
- The state is managed for you. You don't have to write your own code to save state information for a long-running function.

#### Durable Function Types

You can use three durable function types: Client, Orchestrator, and Activity.

**Client functions** are the entry point for creating an instance of a Durable Functions orchestration. They can run in response to an event from many sources, such as a new HTTP request arriving, a message being posted to a message queue, an event arriving in an event stream. You can write them in any of the supported languages.

**Activity functions** are the basic units of work in a durable function orchestration. An activity function contains the actual work performed by the tasks being orchestrated.

These are the functions that do the real work. An activity is a job that you need your workflow to do. For example, you may need your code to send a document to a content reviewer before other activity can publish the document, or you need to create a shipment order to send products to a client.

**Orchestrator functions** describe how actions are executed, and the order in which they are run. You write the orchestration logic in code (C# or JavaScript).

#### Application Patterns for Durable Functions

![1](df1.PNG)
![2](df2.PNG)
![3](df3.PNG)
![4](df4.PNG)
![5](df5.PNG)

#### Durable Functions vs Logic Apps

With Azure Durable Functions, you develop orchestrations by writing code and using the Durable Functions extension.

With Logic Apps, you create orchestrations by using the design surface or editing configuration files.

## Azure CLI commands to create web app service and web app

```
az appservice plan create \
--resource-group azuremolchapter11 \
--name appserviceeastus \
--location eastus \
--sku S1

az webapp create \
--resource-group azuremolchapter11 \
--name azuremoleastus \
--plan appserviceeastus \
--deployment-local-git

```

---

## Azure Storage Queues, Azure Event Hubs, Azure Event Grid, Azure Service Bus

#### Messages vs Events

##### Messages

- A message contains raw data, produced by one component, that will be consumed by another component.
- A message contains the data itself, not just a reference to that data.
- The sending component expects the message content to be processed in a certain way by the destination component. The integrity of the overall system may depend on both sender and receiver doing a specific job.

##### Events

Events are the data messages passing through Event Grid that describe what has taken place. Each event is self-contained, can be up to 64 KB, and contains several pieces of information based on a schema defined by Event Grid

- Events are lighter weight than messages, and are most often used for broadcast communications. The components sending the event are known as publishers, and receivers are known as subscribers.
- With events, receiving components will generally decide in which communications they are interested, and will "subscribe" to those events.
- The subscription is managed by an intermediary, like Azure Event Grid or Azure Event Hubs.

Characteristics:

- An event is a lightweight notification that indicates that something happened.
- The event may be sent to multiple receivers, or to none at all.
- Events are often intended to "fan out," or have a large number of subscribers for each publisher.
- The publisher of the event has no expectation about the action a receiving component takes.
- Some events are discrete units and unrelated to other events.
- Some events are part of a related and ordered series.

#### Azure Queue Storage

- Queue storage is a service that uses Azure Storage to store large numbers of messages that can be securely accessed from anywhere in the world using a simple REST-based interface. Queues can contain millions of messages, limited only by the capacity of the storage account that owns it.

NOTE: The delete request for each Azure Queue Storage message is generated by the consumer. When the Queue sends a message to a client, it waits 30 seconds for a succes response from the client, during which time the message is temporarily hidden from other clients. If a success response is received within 30 seconds, the message is deleted from the Queue, else the message comes back to the Queue and is sent to a different client.

##### Message Delivery gaurantees

- **At-Least-Once Delivery**: In this approach, each message is guaranteed to be delivered to at least one of the components that retrieve messages from the queue. Note, however, that in certain circumstances, it is possible that the same message may be delivered more than once.
- **At-Most-Once Delivery**: In this approach, each message is not guaranteed to be delivered, and there is a very small chance that it may not arrive. However, unlike At-Least-Once delivery, there is no chance that the message will be delivered twice. This is sometimes referred to as "automatic duplicate detection".
- **First-In-First-Out (FIFO)**: In most messaging systems, messages usually leave the queue in the same order in which they were added, but you should consider whether this order is guaranteed. If your distributed application requires that messages are processed in precisely the correct order, you must choose a queue system that includes a FIFO guarantee.

##### Transaction Support

Messages in a Queue can be grouped into a transaction, so that all messages succeed or fail as a single unit.

#### Azure Service Bus Queues

- Service Bus is a message broker system intended for enterprise applications. These apps often utilize multiple communication protocols, have different data contracts, higher security requirements, and can include both cloud and on-premises services. Service Bus is built on top of a dedicated messaging infrastructure designed for exactly these scenarios.

#### Azure Service Bus Topics

- Azure Service Bus topics are like queues, but can have multiple subscribers. When a message is sent to a topic instead of a queue multiple components can be triggered to do their work.

Use Storage queues when you want a simple and easy-to-code queue system. For more advanced needs, use Service Bus queues. If you have multiple destinations for a single message, but need queue-like behavior, use topics.

#### Choose Service Bus Topics if

- you need multiple receivers to handle each message

#### Choose Service Bus queues if

- You need an At-Most-Once delivery guarantee.
- You need a FIFO guarantee.
- You need to group messages into transactions.
- You want to receive messages without polling the queue.
- You need to provide a role-based access model to the queues.
- You need to handle messages larger than 64 KB but less than 256 KB.
- Your queue size will not grow larger than 80 GB.
- You would like to be able to publish and consume batches of messages.

#### Choose Queue storage if:

- You need an audit trail of all messages that pass through the queue.
- You expect the queue to exceed 80 GB in size.
- You want to track progress for processing a message inside of the queue.

## Azure Event Grid

Azure Event Grid is a fully-managed event routing service running on top of Azure Service Fabric. Event Grid distributes events from different sources, such as Azure Blob storage accounts or Azure Media Services, to different handlers, such as Azure Functions or Webhooks.

There are several concepts in Azure Event Grid that connect a source to a subscriber:

- Events: What happened.
- Event sources: Where the event took place.
- Topics: The endpoint where publishers send events.
- Event subscriptions: The endpoint or built-in mechanism to route events, sometimes to multiple handlers. Subscriptions are also used b- handlers to filter incoming events intelligently.
- Event handlers: The app or service reacting to the event.

### Use Event Grid when you need these features:

- Simplicity: It is straightforward to connect sources to subscribers in Event Grid.
- Advanced filtering: Subscriptions have close control over the events they receive from a topic.
- Fan-out: You can subscribe to an unlimited number of endpoints to the same events and topics.
- Reliability: Event Grid retries event delivery for up to 24 hours for each subscription.
- Pay-per-event: Pay only for the number of events that you transmit.

what if we want to deliver a large stream of events? In this scenario, Event Grid isn't a great solution because it's designed for one-event-at-a-time delivery. Instead, we need to turn to another Azure service: Event Hubs.

## Azure Event Hub

Event Hubs is an intermediary for the publish-subscribe communication pattern. Unlike Event Grid, however, it is optimized for extremely high throughput, a large number of publishers, security, and resiliency.

Event Hubs lets you build a big data pipeline capable of processing millions of events per second with low latency. It can handle data from concurrent sources and route it to a variety of stream-processing infrastructures and analytics services. It enables real-time processing and supports repeated replay of stored raw data.

Publishers can use either HTTPS or AMQP (Advanced Message Queuing Protocol). AMQP opens a socket and can send multiple messages over that socket.

Event Hubs default to 4 partitions. Partitions are the buckets within an Event Hub. Each publication will go into only one partition. Each consumer group may read from one or more than one partition.

The maximum size for a single publication (individual or batch) that is allowed by Azure Event Hub is 1 MB.

### Choose Event Hubs if:

- You need to support authenticating a large number of publishers.
- You need to save a stream of events to Data Lake or Blob storage.
- You need aggregation or analytics on your event stream.
- You need reliable messaging or resiliency.

Otherwise, if you need a simple event publish-subscribe infrastructure, with trusted publishers (for instance, your own web server), you should choose Event Grid.

---

## How to choose a communications technology

### Consider the following questions:

- Is the communication an event? If so, consider using Event Grid or Event Hubs.

- Should a single message be delivered to more than one destination? If so, use a Service Bus topic. Otherwise, use a queue.

#### If you decide that you need a queue:

Choose Service Bus queues if:

- You need an at-most-once delivery guarantee
- You need a FIFO guarantee
- You need to group messages into transactions
- You want to receive messages without polling the queue
- You need to provide role-based access to the queues
- You need to handle messages larger than 64 KB but smaller than 256 KB
- Your queue size will not grow larger than 80 GB
- You would like to be able to publish and consume batches of messages

#### Choose queue storage if:

- You need a simple queue with no particular additional requirements
- You need an audit trail of all messages that pass through the queue
- You expect the queue to exceed 80 GB in size
- You want to track progress for processing a message inside of the queue

Although the components of a distributed application can communicate directly, you can often increase the reliability of that communication by using an intermediate communication platform such as Azure Service Bus or Azure Event Grid.

Event Grid is designed for events, which notify recipients only of an event and do not contain the raw data associated with that event. Azure Event Hubs is designed for high-flow analytics types of events. Azure Service Bus and storage queues are for messages, which can be used for binding the core pieces of any application workflow.

If your requirements are simple, if you want to send each message to only one destination, or if you want to write code as quickly as possible, a storage queue may be the best option. Otherwise, Service Bus queues provide many more options and flexibility.

---

Questions:

1. Which of the following queues should you use if you need first-in-first-out order and support for transactions?

- Azure Service Bus queues
- Azure Storage queues

Answer: Even though a queue is a first-in-first-out data structure, Azure Storage queues do not guarantee it. Azure Service Bus queues handle messages in the same order they're added and also support transactions. This means that if one message in a transaction fails to be added to the queue, all messages in the transaction will not be added.

2. Suppose you're sending a message with Azure Service Bus and you want multiple components to receive it. Which Azure Service Bus exchange feature should you use?

- Queue
- Topic
- Relay

Answer: A queue can only have one destination component at a time, which means that each message in the queue is delivered to only one receiver. A relay is used for two-way communication and it provides bidirectional connections across network boundaries. A topic allows multiple destination components to subscribe. This means that each message can be delivered to multiple receivers.

- An Azure Service Bus queue message must be larger than 64 KB but smaller than 256 KB.

---

## Azure Cosmos DB

- Cosmos DB allows us to store our data using Key-Value, Column-Family, Documents or Graph approaches

- APIs : SQL, Table, Cassandra, MongoDB, Gremlin

### Partitioning Schemes

Azure places the data in cosmos DB in different server to accommodate performance and throughput, which is dones using partitions.

Two different types of Partitions

- Logical - partition on our custom partition criteria (partition key)
- Physical - group of replicas of our data physically stored on the servers.

Logical Partition has a limit of 20GB for storing data.

Logical Partition also have an upper throuhput limit.

Choosing the correct partition key is critical for achieving the best performance. The reason choosing the proper partition key is so important is because Azure creates a logical partition for each distinct value of your partition key.

Using the id property as the partition key means that you end with a logical partition with a single document on each partition. This configuration can be beneficial when your application usually performs read workloads and uses parallelization techniques for getting the data.

On the other hand, if you select a partition key with just a few possible values, you can end with “hot” partitions. A “hot” partition is a partition that receives most of the requests when working with your data. The main implication for these “hot” partitions is that they usually reach the throughput limit for the partition, which means you need to provision more throughput.

partition. The minimum throughput limit is different from databases to containers. The minimum throughput for databases is 100 request units per second (RU/s). The minimum throughput for containers is 400 RU/s.

Choose partition keys with a wide range of values and access patterns that can evenly distribute requests across logical partitions. This allows you to achieve the right balance between being able to execute cross-document transactions and scalability. Using timestamp-based partition keys is usually a lousy choice for a partition key.

### Consistency Levels

Replication of Data across regions (one of the advantage of Cosmos DB) has a drawback - the consistency of our data.

To avoid corruption, your data needs to be consistent between all copies of your database. Fortunately, the Cosmos DB protocol offers five levels of consistency replication.

Going from consistency to performance, you can select how the replication protocol behaves when copying your data between all the replicas that are configured across the globe.

- **Strong** - The read operations are guaranteed to return the most recently committed version of an element; that is, the user always reads the latest committed write. This consistency level is the only one that offers a linearizability guarantee. This guarantee comes at a price. It has higher latency because of the time needed to write operation confirmations, and the availability can be affected during failures.

- **Bounded Staleness** - The reads are guaranteed to be consistent within a preconfigured lag. This lag can consist of a number of the most recent (K) versions or a time interval (T). This means that if you make write operations, the read of these operations happens in the same order but with a maximum delay of K versions of the written data or T seconds since you wrote the data in the database. For reading operations that happen within a region that accepts writes, the consistency level is identical to the Strong consistency level. This level is also known as “time-delayed linearizability guarantee.”

- **Session Scoped** - to a client session, this consistency level offers the best balance between a strong consistency level and the performance provided by the eventual consistency level. It best fits applications in which write operations occur in the context of a user session.

- **Consistent Prefix** - This level guarantees that you always read data in the same order that you wrote the data, but there’s no guarantee that you can read all the data. This means that if you write “A, B, C” you can read “A”, “A, B” or “A, B, C” but never “A, C” or “B, A, C.”

- **Eventual** - There is no guarantee for the order in which you read the data. In the absence of a write operation, the replicas eventually converge. This consistency level offers better performance at the cost of the complexity of the programming. Use this consistency level if the order of the data is not essential for your application.

The recommended option for most applications is the level of session consistency.

If you are considering the strong consistency level, we recommend that you use the bonded staleness consistency level because it provides a linearizability guarantee with a configurable delay.

If you are considering the eventual consistency level, we recommend that you use the consistent prefix consistency level because it provides comparable levels of availability and latency with the advantage of guaranteed read orders.

Carefully evaluate the strong and eventual consistency levels because they are the most extreme options. In most situations, other consistency levels can provide a better balance between performance, latency, and data consistency.

---

## Azure Blob Storage

### Moving Blob items between containers

1. Create a BlobServiceClient instance for each Storage Account that is involved in the blob item movement.
2. Create a reference for each container. If you need to move a blob item between containers in a different Storage Account, you need to use the BlobServiceClient object that represents each Storage Account.
3. Create a reference for each blob item. You need a reference to the source blob item because this is the item that you are going to move. You use the destination blob item reference for performing the actual copy operation.
4. Once you are done with the copy, you can delete the source blob item by using the DeleteAsync() method.

NOTE: When you need to move a blob to any destination, container, or Storage Account, remember that you need first to perform a copy operation and then delete the source blob. There is no such move method in the CloudBlockBlob class.

### Concurrency issues in Blob Storage and Leasing Blobs

When you are working with the Blob Storage service — in which several users or processes can simultaneously access the same Storage Account — you can face a problem when two users or processes are trying to access the same blob.

Azure provides a leasing mechanism for solving this kind of situation.

A lease is a short block that the blob service sets on a blob or container item for granting exclusive access to that item. When you acquire a lease to a blob, you get exclusive write and delete access to that blob. If you acquire a lease in a container, you get exclusive delete access to the container.

Leasing is the mechanism that you need to use to ensure that no other users or processes can access a blob while you are working with it. You can create timed or infinite leases. Remember that you need to release infinite leases manually.

When you acquire a lease for a storage item, you need to include the active lease ID on each write operation that you want to perform on the blob with the lease. You can choose the duration for the lease time when you request it. This duration can last from 15 to 60 seconds or forever.

Each lease can be in one of the following five states:
• **Available** The lease is unlocked, and you can acquire a new lease.
• **Leased** There is a lease granted to the resource, and the lease is locked. You can acquire a new lease if you use the same ID that you got when you created the lease. You can also release, change, renew, or break the lease when it is in this status.
• **Expired** The duration configured for the lease has expired. When you have a lease on this status, you can acquire, renew, release, or break the lease.
• **Breaking** You have broken the lease, but it’s still locked until the break period expires. In this status, you can release or break the lease.
• **Broken** The break period has expired, and the lease has been broken. In this status, you can acquire, release, and break a lease. You need to break a lease when the process that acquired the lease finishes suddenly, such as when network connectivity issues or any other condition results in the lease not being released correctly. In these situations, you may end up with an orphaned lease, and you cannot write or delete the blob with the orphaned lease. In this situation, the only solution is to break the lease. You may also want to break a lease when you need to force the release of the lease manually.

if some other process or user modifies the blob while our process is copying the data, we get an error. You can avoid that situation by acquiring a lease for the blob that you want to move.

An infinite lease is created by using a value of -1 for TimeSpan

### Blob Storage Tiers, data archiving and retention

These different access levels, or tiers, provide different levels of performance when accessing the data. Each different access level has a different price.

- **Hot** - You use this tier for data that you need to access more frequently. This is the default tier that you use when you create a new Storage Account.
- **Cool** - You can use this tier for data that is less frequently accessed and is stored for at least 30 days.
- **Archive** You use this tier for storing data that is rarely accessed and is stored for at least 180 days. This access tier is available only at the blob level. You cannot configure a Storage Account with this access tier.

* Archive storage is offline storage. It has the lowest storage cost rates but has higher access costs.
* The lower the storage costs, the higher the access costs.
* You can use storage tiering only on General Purpose v2 (GPv2) Storage Accounts.

Moving between the different access tiers is a transparent process for the user, but it has some implications in terms of pricing.

In general, when you are moving from a warmer tier to a cooler tier—hot to cool or hot to archive—you are charged for the write operations to the destination tier.

When you move from a cooler tier to a warmer tier—from the archive to cold or from cold to hot—you are charged for the read operations from the source tier.

Another essential thing to bear in mind is how the data is moved when you change your data tier from archive to any other access tier. Because data in the archive tier is saved into offline storage, when you move data out of the access tier, the storage service needs to move the data back to online storage. This process is known as blob rehydration and can take up to 15 hours.

Instead of manually monitoring the different criteria for moving a blob from one tier to another, you can implement **policies** that make that movement based on the criteria that you define.

A **lifecycle management policy** is a JSON document in which you define several rules that you want to apply to the different containers or blob types. Each rule consists of a filter set and an action set.

- **Filter set** The filter set limits the actions to only a group of items that match the filter criteria.
- **Action set** You use this set to define the actions that are performed on the items that matched the filter.
