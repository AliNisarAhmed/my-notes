# Optimistic Concurrency Control (or Optimistic Locking)

From [this SO thread](https://stackoverflow.com/questions/129329/optimistic-vs-pessimistic-locking) 

[Optimistic Locking](http://en.wikipedia.org/wiki/Optimistic_locking) is a strategy where you read a record, take note of a version number (other methods to do this involve dates, timestamps or checksums/hashes) and check that the version hasn't changed before you write the record back. When you write the record back you filter the update on the version to make sure it's atomic. (i.e. hasn't been updated between when you check the version and write the record to the disk) and update the version in one hit.

If the record is dirty (i.e. different version to yours) you abort the transaction and the user can re-start it.

This strategy is most applicable to high-volume systems and three-tier architectures where you do not necessarily maintain a connection to the database for your session. In this situation the client cannot actually maintain database locks as the connections are taken from a pool and you may not be using the same connection from one access to the next.


Optimistic locking doesn't necessarily use a version number. Other strategies include using (a) a timestamp or (b) the entire state of the row itself. The latter strategy is ugly but avoids the need for a dedicated version column, in cases where you aren't able to modify the schema.

if your application is mainly a read-only application by a lot of users, and sometimes you write data, than go for optimistic locking. StackOverflow, for example, have a lot of people reading pages, and sometimes someone edit one: in pessimistic locking, who would get the lock? the first one? In optimistic locking, the person who wish to edit the page can do it as long as he have the last version of it.



Optimistic concurrency control (or optimistic locking) assumes that although conflicts are possible, they will be very rare. Instead of locking every record every time that it is used, the system merely looks for indications that two users actually did try to update the same record at the same time. If that evidence is found, then one user's updates are discarded and the user is informed.

For example, if User1 updates a record and User2 only wants to read it, then User2 simply reads whatever data is on the disk and then proceeds, without checking whether the data is locked. User2 might see slightly out-of-date information if User1 has read the data and updated it, but has not yet committed the transaction.


# Pessimistic Locking

[Pessimistic Locking](http://en.wikipedia.org/wiki/Lock_%28database%29) is when you lock the record for your exclusive use until you have finished with it. It has much better integrity than optimistic locking but requires you to be careful with your application design to avoid [Deadlocks](http://en.wikipedia.org/wiki/Deadlock). To use pessimistic locking you need either a direct connection to the database (as would typically be the case in a [two tier client server](http://en.wikipedia.org/wiki/Client-server) application) or an externally available transaction ID that can be used independently of the connection.

In the latter case you open the transaction with the TxID and then reconnect using that ID. The DBMS maintains the locks and allows you to pick the session back up through the TxID. This is how distributed transactions using two-phase commit protocols (such as [XA](http://www.opengroup.org/bookstore/catalog/c193.htm) or [COM+ Transactions](http://msdn.microsoft.com/en-us/library/ms687120%28VS.85%29.aspx)) work.



Pessimistic concurrency control (or pessimistic locking) is called "pessimistic" because the system assumes the worst — it assumes that two or more users will want to update the same record at the same time, and then prevents that possibility by locking the record, no matter how unlikely conflicts actually are.

The locks are placed as soon as any piece of the row is accessed, making it impossible for two or more users to update the row at the same time. Depending on the lock mode (shared, exclusive, or update), other users might be able to read the data even though a lock has been placed. For more details on the lock modes, see [Lock modes: shared, exclusive, and update](https://support.unicomsi.com/manuals/soliddb/7/SQL_Guide/5_ManagingTransactions.06.5.html#ww1126603 "Locks and lock modes").