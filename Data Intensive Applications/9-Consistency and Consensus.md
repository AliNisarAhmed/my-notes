# Consistency & Consensus

In this chapter we examined the topics of consistency and consensus from several different angles.

## Consistency Gaurantees

Most replicated databases provide at least _eventual consistency_, which means that if you stop writing to the database and wait for some unspecified length of time, then eventually all read requests will return the same value.

However, this is a very weak guarantee—it doesn’t say anything about when the replicas will converge.

Eventual consistency is hard for application developers because it is so different from the behavior of variables in a normal single-threaded program.
- When working with a database that provides only weak guarantees, you need to be constantly aware of its limitations and not accidentally assume too much.
- Bugs are often subtle and hard to find by testing

In this chapter we will explore stronger consistency models that data systems may choose to provide. They don’t come for free: systems with stronger guarantees may have worse performance or be less fault-tolerant than systems with weaker guarantees.


## Linearizability

AKA
- atomic consistency
- strong consistency
- immediate consistency
- external consistency

The goal of linearizability is to make replicated data appear as though there were only a single copy, and to make all operations act on it atomically


In a linearizable system, as soon as one client successfully completes a write, all clients reading from the database must be able to see the value just written. 

Maintaining the illusion of a single copy of the data means guaranteeing that the value read is the most recent, up-to-date value, and doesn’t come from a stale cache or replica. 

In other words, linearizability is a _recency guarantee_. To clarify this idea, let’s look at an example of a system that is not linearizable.


![a42fed4b7a294b5912683ee61bedad03.png](./images/a42fed4b7a294b5912683ee61bedad03.png)

If Alice and Bob had hit reload at the same time, it would have been less surprising if they had gotten two different query results, because they wouldn’t know at exactly what time their respective requests were processed by the server. 
- However, Bob knows that he hit the reload button (initiated his query) _after_ he heard Alice exclaim the final score, and therefore he expects his query result to be at least as recent as Alice’s. 
- The fact that his query returned a stale result is a violation of linearizability.


Lets look at another example to see what makes a system Linearizable:

![65d941eb60da00d1896d673170d716d4.png](../images/65d941eb60da00d1896d673170d716d4.png)


Note:
- bar = request by a client
- start of bar = time when request was sent
- end of bar   = time when response was received
- A client does not know when the DB processed its read/write request
- `read(x) => v` = request to read value `x`, response by db = `v`
- `write(x,v) => r` = request to set the register `x` to value `v`, and the database returned `r` = `ok | error`
- initial value of `x` = 0


- First read by A completes before write begins, so return value 0
- last read by A begins after the write, so must return value 1 if the DB is linearizable
- Any reads that overlap in time with the write operation might return 0 or 1. These operations are _concurrent_
- The above scenario is not sufficient to describe linearizability (if reads that are concurrent with a write can return either the old or the new value, then readers could see a value flip back and forth between the old and the new value several times while a write is going on)


![615d0692c8d9089cffb0f6c075141c47.png](../images/615d0692c8d9089cffb0f6c075141c47.png)


We can further refine this timing diagram to visualize each operation taking effect atomically at some point in time, in the diagram below:


![1518d85f8d4b2aa73576e465beb7d46f.png](../images/1518d85f8d4b2aa73576e465beb7d46f.png)

The above diagram introduces another operation
- `cas(x, v_old, v_new) => r` = client requested atomic compare-and-set. If the current value == `v_old`, set it to `v_new` and return `ok`, else return `error`

Notes from diagram above:
- First client B sent a request to read x, then client D sent a request to set x to 0, and then client A sent a request to set x to 1. Nevertheless, the value returned to B’s read is 1 (the value written by A). This is okay: it means that the database first processed D’s write, then A’s write, and finally B’s read.
- Client B’s read returned 1 before client A received its response from the database, saying that the write of the value 1 was successful. This is also okay: it doesn’t mean the value was read before it was written, it just means the ok response from the database to client A was slightly delayed in the network.
- The final read by client B (in a shaded bar) is not linearizable. The operation is concurrent with C’s CAS write, which updates x from 2 to 4. In the absence of other requests, it would be okay for B’s read to return 2. However, client A has  already read the new value 4 before B’s read started, so B is not allowed to read an older value than A.

It is possible (though computationally expensive) to test whether a system’s
behavior is linearizable by recording the timings of all requests and responses, and checking whether they can be arranged into a valid sequential order.