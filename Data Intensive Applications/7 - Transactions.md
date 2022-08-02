# Transactions

A transaction is a way for an application to group several reads and writes together into a logical unit. 

Conceptually, all the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds (_commit_) or it fails (_abort_, _rollback_).

Almost all relational databases today, and some nonrelational databases, support transactions.

## The Meaning of ACID

The safety guarantees provided by transactions are often described by the well-known acronym _ACID_, which stands for _Atomicity_, _Consistency_, _Isolation_, and _Durability_.

However, in practice, one database’s implementation of ACID does not equal another’s implementation.
- e.g. there is a lot of ambiguity around the definition of _isolation_

### Atomicity

In general, _atomic_ refers to something that cannot be broken down into smaller parts.

The word means similar but subtly different things in different branches of computing. 
- For example, in multi-threaded programming, if one thread executes an atomic operation, that means there is no way that another thread could see the half-finished result of the operation. 
    - The system can only be in the state it was before the operation or after the operation, not something in between.

By contrast, in the context of ACID, atomicity is _not_ about concurrency. 
- It does not describe what happens if several processes try to access the same data at the same time, because that is covered under the letter _I_, for _isolation_

Rather, ACID atomicity describes what happens if a client wants to make several writes, but a fault occurs after some of the writes have been processed, for example, a process crashes, a network connection is interrupted, a disk becomes full, or some integrity constraint is violated.  
- If the writes are grouped together into an atomic transaction, and the transaction cannot be completed (_committed_) due to a fault, then the transaction is _aborted_ and the database must discard or undo any writes it has made so far in that transaction.
- if a transaction was aborted, the application can be sure that it didn’t change anything, so it can safely be retried.

### Consistency

The idea of ACID consistency is that you have certain statements about your data (_invariants_) that must always be true—for example, in an accounting system, credits and debits across all accounts must always be balanced. 
- If a transaction starts with a database that is valid according to these invariants, and any writes during the transaction preserve the validity, then you can be sure that the invariants are always satisfied.

However, this idea of consistency depends on the application’s notion of invariants, and it’s the application’s responsibility to define its transactions correctly so that they preserve consistency. 
- This is not something that the database can guarantee

Atomicity, isolation, and durability are properties of the database, whereas consistency (in the ACID sense) is a property of the application.
- Thus, the letter C does not really belong in ACID

### Isolation

Most databases are accessed by several clients at the same time. That is no problem if they are reading and writing different parts of the database, but if they are accessing the same database records, you can run into concurrency problems (race conditions).

_Isolation_ in the sense of ACID means that concurrently executing transactions are isolated from each other: they cannot step on each other’s toes.

The classic database textbooks formalize isolation as _serializability_, which means that each transaction can pretend that it is the only transaction running on the entire database. The database ensures that when the transactions have committed, the result is the same as if they had run _serially_ (one after another), even though in reality they may have run concurrently.

However, in practice, serializable isolation is rarely used, because it carries a performance penalty.

![32209530efed2939645e57628fcc3018.png](images/32209530efed2939645e57628fcc3018.png)

### Durability

The purpose of a database system is to provide a safe place where data can be stored without fear of losing it. _Durability_ is the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes.

In a single-node database, durability typically means that the data has been written to nonvolatile storage such as a hard drive or SSD. It usually also involves a write-ahead log or similar, which allows recovery in the event that the data structures on disk are corrupted. 

In a replicated database, durability may mean that the data has been successfully copied to some number of nodes.

In order to provide a durability guarantee, a database must wait until these writes or replications are complete before reporting a transaction as successfully committed.

Perfect durability does not exist: if all your hard disks and all your backups are destroyed at the same time, there’s obviously nothing your database can do to save you.