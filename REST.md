# RESTful services

## HTTP Head Method

- HTTP HEAD is identical to HTTP GET, with the notable difference that the API should not return a response body.
- The HTTP HEAD method requests the headers that are returned if the specified resource would be requested with an HTTP GET method.
- Such a request can be done before deciding to download a large resource to save bandwidth, for example. A response to a HEAD method should not have a body.

## REST Guidelines

- Some of the most important concerns that a RESTful architecture affects include performance, scalability, simplicity, interoperability, communication visibility, component portability, and reliability. These properties are encapsulated by six principles, which are defined by Roy Fielding (2000) as the constraints guiding a RESTful system design.

1. **Client-Server Constraint** - The Client-Server constraint enforces the proper separation of concerns between the UI/consumer and the back-end, which mostly contains the business-logic and data-storage implementations. Enforcing separation between the client and the server
   promotes the ability to have them evolve entirely independently from each other given that the interface between them doesn’t change.
2. **Layered System constraint** - dictates that layers should be organized hierarchically, restricting the use of a service to the layers directly beneath and above it. Orchestrating components in layers drastically improves reusability, making them more modular.
3. **Stateless Constraint** - Building on the Client-Server style is the Stateless constraint. Communication between the client and the server needs to be stateless, meaning that a request should contain all the information necessary for the server to understand and to create context. The client is ultimately responsible for managing session state and cannot rely on the server for directly storing any state data. Does this mean that the actual contents of the state need to be transferred back and forth all the time? The short answer is no—it is entirely acceptable for the state to be persisted elsewhere and for the client to include an identifier for retrieving it.
4. **Uniform Interface** - The key feature that associates a system with REST is a Uniform Interface. This constraint consists of four essential parts, which are resource identification, resource manipulation, self-describing responses, and state management. These architectural elements are implemented directly through URIs, HTTP verbs, media types, and Hypermedia as the Engine of Application State (HATEOAS), respectively.
5. **Cache Contraint** - The Cache constraint derives from the Stateless constraint and requires that responses coming from the server are explicitly labeled as cacheable or non-cacheable, regardless if they are explicitly or implicitly defined. Responses that are cached allow clients to reuse them later when making similar requests, thus improving speed and latency. Caching can be applied to both the client and the server side.
   **_Caution_** It is important to not confuse response cache and application/session state, which can be wrongly interpreted as conflicting constraints of REST. The former refers to short-lived transactional messages, and the latter denotes specific persisted context.
6. **Code on Demand** - The final and optional constraint is Code on Demand, which allows a client to access specific resources from the server without knowledge of how to process them. This style is typically implemented by web-based applications that have clients using a client-side scripting language, like JavaScript. Having the ability to add functionality to a deployed client not only promotes extensibility but can also help to offload some serverside tasks onto the client, making it more responsive.

Apply these six constraints to your API services—then and only then will they become truly RESTful.

### Http Errors vs Faults

- **Errors** occur when the consumer of the API passes invalid data to the API, and the API correctly rejects this.
  - Mostly conveyed by **400** HTTP Status codes.
  - Do not contribute to overall API availability
- **Faults** OTOH occur when the API fails to return a response to a valid request.
  - Conveyed by **500** status codes
  - Do contribute to overall API availability

### Rest conventions

- For validation errors, it is best to return a 422 status code
- 500 internet server error is used for Faults (that is, something going wrong not because of client's input)
- 404 not found, or 403 bad request are used in case of client errors (and not in faults)

- A **PUT** request is Idempotent, while a **POST** and **PATCH** request are not.
- A **PATCH** required a **patch document** to specify how to carry out the changes

  - A **patch document** is a subset of 6 instructions specified in JSON format
  - the instructions are `add` `remove` `replace` `copy` `move` `test` (check if a value exists)

- A **DELETE** request results in 204 no content code.
- **DELETE** HTTP method is idempotent
