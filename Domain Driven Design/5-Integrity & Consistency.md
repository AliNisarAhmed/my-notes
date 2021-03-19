# Integrity & Consistency 

## Integrity 

Integrity means a piece of data follows the correct business rules. For example

- a UnitQuantity is between 1 and 1000. Do we have to check this multiple times in our code, or can we rely on this to always be true?
- An order must always have at least one order line.
- An order must have a validated shipping address before being sent to the shipping department.

## Consistency 

Consistency here means that different parts of the domain model agree about facts. Examples: 

- The total amount to bill for an order should be the sum of the individual lines. If the total differs, the data is inconsistent.
- When an order is placed, a corresponding invoice must be created. If the order exists but the invoice doesn’t, the data is inconsistent.
- If a discount voucher code is used with an order, the voucher code must be marked as used so it can’t be used again. If the order references that
voucher but the voucher is not marked as used, the data is inconsistent.

Consistency is a business term, not a technical one, and what consistency means is always context-dependent. 

For example, 
    if a product price changes, should any unshipped orders be immediately updated to use the new price? What if the default address of a customer changes? 
    Should any unshipped orders be immediately updated with the new address? 
    
There’s no right answer to these questions—it depends on what the business needs.

### Consistency within a Single Aggregate

Let’s say that we require that the total amount for an order should be the sum of the individual lines. 

There are two ways to ensure consistency.

1. 

The easiest way to ensure consistency is simply to calculate information from the raw data rather than storing it. 

In this case then, we could just sum the order lines every time we need the total, either in memory or using a SQL query.

2. 
If we do need to persist the extra data (say an additional AmountToBill stored in the top-level Order ), then we need to ensure that it stays in sync. 

In this case then, if one of the lines is updated, the total must also be updated in order to keep the data consistent. It’s clear that the only component that “knows” how to preserve consistency is the top-level Order . This is a good reason for oing all updates at the order level rather that at the line level—the order is the aggregate that enforces a consistency boundary.

Aggregates are also the unit of atomicity, so if we save this order to a relational database, say, we must ensure that the order header and the order lines are all inserted or updated in the same transaction.


### Consistency between different Contexts

What if we need to coordinate between different contexts?

- When an order is placed, a corresponding invoice must be created. If the order exists but the invoice doesn’t, the data is inconsistent.

Invoicing is part of the billing domain, not the order-taking domain. 

Does that mean we need to reach into the other domain and manipulate its objects No, of course not. We must keep each bounded context isolated and decoupled.

One approach is to use the billing Context's public API, like this: 

```
Ask billing context to create invoice
If successfully created:
    create order in order-taking context
```

This approach is much trickier than it might seem, because you need to handle either update failing. There are ways to synchronize updates across separate systems properly (such as a two-phase commit), but in practice it’s rare to need this.

In his article [“Starbucks Does Not Use Two-Phase Commit,”](https://www.enterpriseintegrationpatterns.com/ramblings/18_starbucks.html) 
Gregor Hohpe points out that in the real world businesses generally do not require that every process move in lockstep, waiting for all subsystems to finish one stage before moving to the next stage. 

Instead, coordination is done asynchronously using messages. Occasionally, things will go wrong, but the cost of dealing with rare errors is often much less than the cost of keeping everything in sync.

For example, let’s say that instead of requiring an invoice be created immediately, we just send a message (or an event) to the billing domain and then continue with the rest of the order processing.

So now what happens if that message gets lost and no invoice is created?
- One option is to do nothing. Then the customer gets free stuff and the business has to write off the cost. That might be a perfectly adequate solution if errors are rare and the costs are small (as in a coffee shop).
- Another option is to detect that the message was lost and resend it. This is basically what a reconciliation process does: compare the two sets of data, and if they don’t match up, fix the error.
- A third option is to create a compensating action that “undoes” the previous action or fixes the error. In an order-taking scenario, this would be equivalent to cancelling the order and asking the customer to send the items back! More realistically, a compensating action might be used to do things such as correct mistakes in an order or issue refunds.

In all three cases, there’s no need for rigid coordination between the bounded contexts.

If we have a requirement for consistency, then we need to implement the second or third option. But this kind of consistency won’t take effect immediately.

Instead, the system will become consistent only after some time has passed— “eventual consistency.” Eventual consistency is not “optional consistency”: it’s still very important that the system be consistent at some point in the future.

Here’s an example. Let’s say that if a product price has changed, we want to update the price for all orders that haven’t shipped yet. 

If we need immediate consistency, then when we update the price in the product record, we also have to update all affected orders and do all this within the same transaction. This could take some time. 

But if we don’t require instant consistency when a product price has changed, we might instead create a PriceChanged event that in turn triggers a series of UpdateOrderWithChangedPrice commands to update the outstanding orders. These commands will be processed some time after the price in the product record has changed, perhaps seconds later, perhaps hours later. Eventually the orders will be updated and the system will be consistent.

### Consistency between Aggregates in the same context

What about ensuring consistency between aggregates in the same bounded context? Let’s say that two aggregates need to be consistent with each other.

Should we update them together in the same transaction or update them separately using eventual consistency? Which approach should we take?

As always, the answer is that it depends. In general, a useful guideline is “only update one aggregate per transaction.” 

If more than one aggregate is involved, we should use messages and eventual consistency as described above, even though both aggregates are within the same bounded context.

But sometimes—and especially if the workflow is considered by the business to be a single transaction—it might be worth including all affected entities in the transaction
