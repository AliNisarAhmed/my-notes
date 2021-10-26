# Consistency Models


A consistency model defines the degree of data consistency, and a wide spectrum of possible consistency models exist:
- Strong consistency: any read operation returns a value corresponding to the result of the most updated write data item. A client never sees out-of-date data.
- Weak consistency: subsequent read operations may not see the most updated value.
- Eventual consistency: this is a specific form of weak consistency. Given enough time, all updates are propagated, and all replicas are consistent.

Strong consistency is usually achieved by forcing a replica not to accept new reads/writes until every replica has agreed on current write. 
    - This approach is not ideal for highly available systems because it could block new operations. Dynamo and Cassandra adopt eventual consistency.
    - From concurrent writes, eventual consistency allows inconsistent values to enter the system and force the client to read the values to reconcile