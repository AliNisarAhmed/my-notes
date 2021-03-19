## Identity: Values vs Entities

in DDD terminology, objects with a persistent identity are called **Entities** and objects without a persistent identity are called **Value Objects**

Value Objects do not have an identity and are interchangeable, etc WidgetCode, or a person's first and last name. Two different persons could have the same first and last names, but they are still two different persons. 

Entities have a unique identity, even if their contents change.
    - e.g. if a person changes their address, they still are the same person

In business contexts, entities are often a document of some kind: orders, quotes, invoices, customer profiles, product sheets and so on.

Entities have a life cycle and are transformed from one state to another by various business processes.



The distinction between “Value Object” and “Entity” is context-dependent.

For example, consider the life cycle of a cell phone.  During manufacturing, each phone is given a unique serial number—a unique identity—so in that context, the phone would be modeled as an Entity. 

When they’re being sold, however, the serial number isn’t relevant—all phones with the same specs are interchangeable—and they can be modeled as Value Objects. 

But once a particular phone is sold to a particular customer, identity becomes relevant again and it should be modeled as an Entity: the customer thinks of it as the same phone even after replacing the screen or battery.


## Aggregates

(basically, Entities embedded inside other Entities)

An Aggregate is a collection of Entities, each with their own identities, and also some top-level Entity that contains them. 

The top level entity is called Aggregate Root. 

Changing in sub-entities always entails change beginning from the Aggregate root. 

Just to be clear, an aggregate is not just any collection of Entities. For example,
a list of Customers is a collection of Entities, but it’s not a DDD “aggregate,” because
it doesn’t have a top-level Entity as a root and it isn’t trying to be a consistency
boundary.


##### Example: 

Consider, an Order consisting of many OrderLines (products)

is Order an Entity? 
    - Entity - because the details of the order may change, but it's still the same order 

is OrderLine an Entity?
    - Yes, even if we change, lets say the quantity, it's still the same Orderline

Now the question is, if we want to make a change to an OrderLine, do we need to make changes to the Order? With Immutable Data Structures, the answer is Yes. 

If I have an immutable Order containing immutable OrderLines , then just making a copy of one of the order lines does not also make a copy
of the Order as well. 

In order to make a change to an OrderLine contained in an Order , I need to make the change at the level of the Order , not at the level of the
OrderLine.

### Aggregates enforce Consistency and Invariants

An aggregate plays an important role when data is updated. The aggregate acts as the consistency boundary: when one part of the aggregate is updated, other parts might also need to be updated to ensure consistency.

For example, we might extend this design to have an additional “total price” stored in the top-level Order . Obviously, if one of the lines changes price, the total must also be updated in order to keep the data consistent.

It’s clear that the only component that “knows” how to preserve consistency is the top-level Order —the aggregate root—so this is another reason for doing all updates at the order level rather than at the line level.

The aggregate is also where any invariants are enforced. Say that you have a rule that every order has at least one order line. Then if you try to delete multiple order lines, the aggregate ensures there is an error when there’s only one line left.

### Aggregates as a unit of persistence 

Aggregates are the basic unit of persistence. If you want to load or save objects from a database, you should load or save whole aggregates. Each database transaction should work with a single aggregate and not include multiple aggregates or cross aggregate boundaries.

##### Example 

Let’s say we need information about the customer to be associated with an Order . The temptation might be to add the Customer as a field of an Order , like this:

```fsharp
type Order = {
    OrderId : OrderId
    Customer : Customer // info about associated customer
    OrderLines : OrderLine list
    // etc
}

```
But think about the ripple effect of immutability. If I change any part of the customer, I must also change the order as well. Is that really what we want?

A much better design is just to store a reference to the customer, not the whole customer record itself. That is, we would just store the CustomerId in the Order type, like this:

```fsharp 
type Order = {
    OrderId : OrderId
    CustomerId : CustomerId // reference to associated customer
    OrderLines : OrderLine list
    // etc
}

```

In this approach, when we need the full information about the customer, we would get the CustomerId from the Order and then load the relevant customer data from the database separately, rather than loading it as part of the order.

In other words, the Customer and the Order are distinct and independent aggregates. They each are responsible for their own internal consistency, and the only connection between them is via the identifiers of their root objects.