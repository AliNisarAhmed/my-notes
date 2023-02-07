# Dynamo DB

- serverless, non-relational db
- a mix of KV and document database model
- provides high throughput and single-digit latency performance
- it scales automatically
- requires zero db admin.
- replicates across multiple AZs within a region
- built-in fault tolerance
- Can also be queried with SQL using `PartiQL` language

![d06a24993dec2007845cd85d580594dd.png](../../../../dev/my-notes-work/images/d06a24993dec2007845cd85d580594dd.png)


## Secondary Index

A data structure that contains a subset of attributes from a table, along with an alternate key to support query operations.

Using secondary index is faster than full table scans

![86d591232b24831dba00f9f79c6554b0.png](../../../../dev/my-notes-work/images/86d591232b24831dba00f9f79c6554b0.png)


Attribute Projection options
1. Only Keys - only the partition key and sort key if any
2. Include - attributes described in ONLY keys + other non-key attributes
3. ALL - all attributes of the base table are projected


### Global Secondary Index (GSI)

Related: [What are WCU & RCU in Amazon Dynamo DB](https://stackoverflow.com/questions/71544613/dynamodb-what-are-wcu-and-rcu)

The scope of queries that we can execute against the GSI is global, as the name suggests. It means the search can span all items in a base table, across all partitions.

GSI can be set up during creation of a table or later.

A table can have up to 20 GSIs (default limit)

a GSI table can have a different partition key and sort key

![57d6a20fce79b25b481eeb50775f1f85.png](../../../../dev/my-notes-work/images/57d6a20fce79b25b481eeb50775f1f85.png)


A GSI has its own provision WCU and RCU, they consume units from the index, and not from the base table.

Writes, however, can. only happen to base table and are later asynchronously replicated to GSIs with eventual consistency