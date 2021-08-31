## Microservices

**Microservices** as opposed to **Monoliths**

* A monolith app contains all of the code required (Routing, Middlewares, Business Logic, DB Access etc) to implement all features of an application.
* A microservice contains all of the code required to implement _one_ feature of our app.


--- 

## Microservices

#### The #1 challenge with Microservices is: Managing data between different services

Microservices store and access data in a different way compared to monoliths

- Each Service gets its own DB (if it needs one)
- One service never, ever reaches into another service's database to access data.

--- 

## Microservices

Why DB per service: 

* We want services to run independently of other services.
* A single DB shared b/w services would be a single point of failure, which would limit the reliability of our app.
* DB schema/structure of one service may change unexpectedly, and we want to eliminate the effects of such changes on other services.
* We can optimize what type of DB we pick for each service, that is, some services might function more efficiently with different types of DB (SQL vs NoSQL)

---

## Sync vs Async Communication between services

NOTE: These two communication strategies have nothing in common with how the code is executed in JS.

1. **Sync:** - services communicate with each other using direct requests (without any intermediary, like an event bus) whatever the form, e.g. http, json api etc.

vs 

2. **Async:** - Using an intermediary like an event-based communication or an event bus


--- 

## Sync 

Advantages 

* conceptually easy to understand
* Most Services won't need a DB, as they can directly request data from other relevant services

Disadvantages

* Introduces a dependency between services.
* If any inter-service request fails, the whole request fails. (Since one service could request to other service which could trigger request to other services and so on)
* The entire request is only as fast as the slowest request
* Can easily introduce a web of requests which can grow very complicated very quickly. 



--- 

## Async Communication between services - Method 1

1. Method 1 (Not very good) - each service connects to a central event bus, where they can emit or receive events.

**Advantages** 

- Most services still do not need their own DB
    
**Disadvantages**

* Conceptually not easy to understand anymore
* Single point of failure (Event bus)
* One failed request fails the whole chain of events
* Again leads to a complicated web of requests/events

--- 

## Async Communication between services - Method 2

- Each service keeps its own DB where it keeps a copy of all the data it needs. 
- Whenever a service gets updated, it emits a relevant event to our service to update its DB with new data,
    - so for example, if a new product is created, an event is emitted by the Product service to our service, to update the list of products.

--- 

### Method 2

**Advantages**
      
- Most services have ZERO dependencies on other services (since they always keep all the relevant data up to date in their own DB)
- Most services therefore will be very fast (as they do not need to shoot events to other services and wait for them to finish)

--- 

### Method 2
      
**Disadvantages**
      
- Data Duplication
    - NOTE: The cost of storing additional data is NOT a disadvantage, as data storage is VERY cheap.
    - By data duplication, we mean complexity of making sure the data is accurate and up to date in all of the copies for all services
- Harder to understand.

--- 

## Notifications in MicroServices

- Do we adopt the philosophy that every microservice should handle their own notifications 
 
OR 

- should all notifications belong to a service unto themselves and rely on database triggers or events?

---

### Database Triggers

Pros

- Considering the volume of notifications being sent out, it is best to have them centralized. 
- Any updates, additions or deletions of notifications should occur in a single place. 
- Furthermore, having notifications sent from the controller is risky, since it is possible that after the action is taken place, the DML operation fails. This would result in a user receiving a notification which is out of sync with the source of truth - the database. 

Cons 

- Opposed to the "philosophy" of microservice architecture.
- If there are multiple services updating a table, you may have multiple notifications going off to users with conflicting entries.

--- 

### Notification as it's own service

We have 3 options: 

1. A queue where all other services send messages to. 

    - The notification microservice would then listen to this queue and send messages accordingly. 
    - If the notification service was restarted or taken down, we would not lose any messages as the messages will be stored on the queue.

--- 

### Notification as it's own service - Option 2

2. Add a notification message API on the notifications microservice. 

    - This would make it easier for all other microservices as they just have to call an API as opposed to integrate with the queue. 
    - The API would then internally send the message to the notification queue and send the message. 
    - The only issue here is if the API is not available or there is an error, we will lose messages.

--- 

### Notification as it's own service - Option 3

3. Have another service that fronts the Queue (separate from the actual service that handles the messages)

    - **Hide the implementation.** This gives you the flexibility to change Queue out later for something else. Your wrapper API would only respond with a 200 once it has put the notification request on the queue. 
    - **Enforce the notification request contract** (ensure the body of the request is well-formed). If we want to make sure that all of the items put on the queue are well-formed according to contract, an API can help enforce that. That will help prevent issues later when the "notifier" service picks notifications off the queue to send.

--- 

### How Option #3 unfolds


1.  Client 1 needs to put out a notification and calls **Service A** with a POST request.
2.  **Service A** then accepts POST request (or rejects it for Contract violation).
3.  **Service A** checks the request, puts it on Queue, responds to the client with 200.
4.  **Service B** picks up notification request from Kafka queue and issues notification.

**Service A** should be run as multiple instances for reliability.

---

The End