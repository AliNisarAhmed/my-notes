
## Output of a workflow: 

**Important point to note:** 

The output of a workflow should always be the events that it generates, the things that trigger actions in other bounded contexts. 

In our case, the output of the workflow would be something like an “OrderPlaced” event, which is then sent to the shipping and billing contexts.

An acknowledgement sent to the customer after an order is placed is not an output of the workflow, but a side effect

It is a side effect because the customer is not a Bounded Context in our domain.

## Persistence Ignorance

Persistence Ignorance means that the domain model should be based only
on the concepts in the domain itself and should not contain any awareness
of databases or other persistence mechanisms.

It is always better to work on the domain and model it without respect to any particular storage implementation. 

The concept of the database is not part of the ubiquitous language. The users do not care about how data is persisted.

This is called persistence ignorance. 

Doing OOP class based design, trying to distort everything in terms of inheritence chains/hierarchies is also to be avoided.


***
