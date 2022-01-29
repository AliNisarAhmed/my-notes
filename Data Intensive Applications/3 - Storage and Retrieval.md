# Simple Database - using append-only log and Hash index

Let's say we have two bash functions
- `db_set key value` stores `key` and `value` in the database
- `db_get key` to get the value

The underlying storage format is simple: 
    - a text file where each line contains a key-value pair, separated by a comma
    - Every call to `db_set` appends to the end of the file
    - So, if we update the key-value several times, the old versions of the value are not overwritter
        - the latest value will be at the later end of the file

The `db_set` function has pretty good performance for something so simple - appending to a file is pretty quick
    - This append only data-file is called a _log_

OTOH, `db_get` has terrible performance
    - basically loop through the whole database and find the last occurrence
    - thus `O(n)`

In order to find the value of a particular key, we need a different structure called an _index_
- An index is additional structure that is derived from the primary data.
- Many databases allow you to add and remove custom indexes
- Adding indexes does not affect the content of the database, it only affects the performance of queries
- Well-chosen indexes speeds up queries, but every index slows down _writes_

### Hash Index

Keep an in-memory hash map where every key is mapped to a byte offset in the data file - the location at which the value can be found

![a18204bbe2330ed55a251f44b7fa23e1.png](a18204bbe2330ed55a251f44b7fa23e1.png)


The hashmap is kept in memory

This type of index is used in BitCask (storage engine in Riak)

A storage engine like BitCask is well suited to the situations where the value for each key is updated frequently
- e.g. The key might be the URL of a cat video, and the value might be the number of times it has been played (incremented every time someone hits the play button)
- In this kind of workload, there are a lot of writes, but there are not too many distinct keys
    - You have a large number of writes per key, but it's feasilbe to keep all keys in memory


### Segments


Question: If we always append to a file, how do we avoid eventually running out of disk space
- A good solution is to break the log into segments of a certain size by closing a segment file when it reaches a certain size
    - and making subsequent writes to a new segment
- We can then perform *compaction* on these segments
- Compaction means throwing away duplicate keys in the log, and keeping only the most recent update for each key


![04c7d52341059a5cb2901389101cd126.png](04c7d52341059a5cb2901389101cd126.png)


Since compaction often makes segments much smaller (assuming that a key is over-written within one segment), we can also merge several segments together at the same time as performing the compaction
- Segments are never modified after they have been written (immutable)
- so the merged segment is written to a new file
- Thus, merging and compaction can be done using a background thread
    - while we continue to serve the read requests using the old segment files
    - and write requests to the latest segment files
- After the merging process is complete, we switch read requests to the new merged segments 
- and then delete the old segment files


![79aaf3133670bffaba2246700de130dc.png](79aaf3133670bffaba2246700de130dc.png)


Each segment has its own in-memory hash table, mapping keys to the file offsets.
- In order to find the value for a key, we first check the most recent segment’s hash map; 
- if the key is not present we check the second-most-recent segment, and so on. 
- The merging process keeps the number of segments small, so lookups don’t need to check many hash maps.

### Issues and solutions 

Some issues and how they are solved: 
- File Format
    - the log often uses binary format (first encodes the length of a string in bytes, followed by raw strings)
- Deleting records
    - to delete a key, we append a special deletion record (called a tombstone)
    - when log segments are merged, the tombstones tell the merging process to discard any previous values for the deleted keys
- Crash Recovery
    - if the database is restarted, the in-memory hash maps are lost
    - We could generate the hash maps again, but that is slow
    - BitCask speeds up recovery by storing a snapshot of each segment's hashmap on disk
- Partially Written records
    - The database may crash at any time, including half-way through appending a record of the log
    - BitCask includes checksums to prevent that
- Concurrency Control
    - a common implementation is to have one writer thread, since appends are in a strictly sequential order
- The hash table must fit in memory, so if you have a very large number of keys, you’re out of luck
    - you could maintain a hash map on disk, but unfortunately it is difficult to make an on-disk hash map perform well.
- Range queries are not efficient
    - e.g., to scan over all keys between `kitty00000` and `kitty99999`, we'd have to look up each key individually in the hash maps


—

# SSTables

In SSTables, the sequence of key-value pairs in log-structured storage is sorted by key
- This is called Sorted String Table

With this format, we cannot append new KV pairs to the segment immediately, since writes can occur in any order.

### Advantages

1. Merging segments is simple and efficient, even if the files are bigger than the available memory
    - you start reading the input files side by side, look at the first key in each file, copy the lowest key (according to the sort order) to the output file, and repeat. This produces a new merged segment file, also sorted by key.
    - When multiple segments contain the same key, we can keep the value from the most recent segment and discard the values in older segments.
2. We no longer need to keep an  index to find a particular key
    - e.g. looking for key `handiwork` in the diagram below, we know it must exist between the known offsets for `handbag` and `handsome`
  
  
![f92619e4a9a5aab44052813f766338cd.png](f92619e4a9a5aab44052813f766338cd.png)


3. Since read requests need to scan over several key-value pairs in the requested range anyway, it is possible to group those records into a block and compress it before writing it to disk (indicated by the shaded area in the above figure)


### Constructing and maintaining SSTables

There are plenty of DS we can use to maintain an SSTable in memory - like red-black tree or AVL tree
    - With these DS, you can insert keys in any order and read them back in sorted order

The Storage engine works as follows:
- When a write comes in, add it to an in-memory balanced tree DS, called a _memtable_
- When the _memtable_ gets bigger than some threshold, typically a few MBs, write it out to a disk as an SSTable
    - This can be done efficiently because the tree already maintains the key value pairs sorted by key.
- In order to serve a read request
    - first try to find the key in _memtable_ 
    - then in the most recent on-disk segment
    - then in the next older one and so on
- From time to time, run a merging and compaction process in the background to combine segment files and to discard overwritten or deleted values

This scheme works very well. It only suffers from one problem: 
- if the database crashes, the most recent writes (which are in the memtable but not yet written out to disk) are lost. 
- In order to avoid that problem, we can keep a separate log on disk to which every write is immediately appended, just like in the previous section. 
- That log is not in sorted order, but that doesn’t matter, because its only purpose is to restore the memtable after a crash. 
- Every time the memtable is written out to an SSTable, the corresponding log can be discarded.