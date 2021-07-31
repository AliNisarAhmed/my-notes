# Postgres Internals


- Query: `SHOW data_directory;` will show the directory where postgres is installed. (e.g. `var/lib/postgresql/12/main`)

Inside the above directory, there is a `/base` directory, where each database is stored in individual folders with unique number indentifiers.

- Query: `SELECT oid, datname FROM pg_database;` will show the list of the database with their ids

![10292e29cb2cb10670f55a86cf1d153e.png](10292e29cb2cb10670f55a86cf1d153e.png)

- Inside each database folders, there are tons of files storing raw data stored inside the database.

Query: `SELECT * FROM pg_class;` shows all these files.

![72cdb7cd89fb0236e986f7b049957596.png](72cdb7cd89fb0236e986f7b049957596.png)


### Heaps, Blocks and Tuples

![8246d04ac4371c77ad80fa0c0c983b81.png](8246d04ac4371c77ad80fa0c0c983b81.png)

![9b7e345703c8362bd3fa26f2933d2f5a.png](9b7e345703c8362bd3fa26f2933d2f5a.png)

Each Block/Page can store 0 or more Items.

Each Block is 8kb large.

![dfb571b3393c5dd61db0e05f2f3feea9.png](dfb571b3393c5dd61db0e05f2f3feea9.png)


![b5d06ee55ff84063188805793cbbd8ea.png](b5d06ee55ff84063188805793cbbd8ea.png)
The right hand side shows the byte map (a representation for actual bits stored on the hard drive) for each block as shown below.

![1db30d07c6cf7e3ba9f3ac447e2e2635.png](1db30d07c6cf7e3ba9f3ac447e2e2635.png)

Documentation on the above page format: https://www.postgresql.org/docs/current/storage-page-layout.html

### Full Table Scans 

Full Table Scans occur when Postgres has to load many (or all) rows from the heap file (stored on HD) in to Memory. 

Full Table scans, frequently, but not always, result in poor performance, and whenever we find a FTS, we should investigate further to find an alternative way of querying data.

Example FTS Query:

![25fe1f9fc12059a5aea44974920c516f.png](25fe1f9fc12059a5aea44974920c516f.png)

![5895c85d31c01c4901b0efb4cd000c53.png](5895c85d31c01c4901b0efb4cd000c53.png)


### Index

Indexes are one way to avoid FTS.

Index are a Data Structure (a B-Tree) that efficiently tells us what block/index a record is stored at.

![12fc59d13440f611b2cca60d310a8a58.png](12fc59d13440f611b2cca60d310a8a58.png)

We can create an index with query: `CREATE INDEX ON users (username);`, this creates an index on `username` column of `users` table.

By default, the index is named in this format: `name of table_name of column_ idx`, e.g `users_username_idx`

To delete an index, we query: `DROP INDEX users_username_idx;`

Postgres creates the following indexes automatically
- Postgres automatically creates an index for the Primary Key column of every table.
- Postgres automatically creates an index for any `unique` constraints.

Note: these auto indexes don't get listed under the `indexes` menu item in PGAdmin. We can use the following query to list all indexes: 

`SELECT relname, relkind FROM pg_class WHERE relkind = 'i';`

Indexing on a column can greatly increase performance, but it comes with its own cost.

1. Since index store a tree data structure for speed, the memory footprint of the database increases. 
2. Indexing slows down insert/update/delete - since the index needs to be updated everytime
3. Index might not actually get used after all - just because an index exist, does not mean the Postgres is actually going to use it. 
4. Some queries are faster without indexes.

Index are of several different types in Postgres (Just be aware of this fact).

![bdd3ce6975437587bd6055dd3b83ac44.png](bdd3ce6975437587bd6055dd3b83ac44.png)


#### Index under the hood

![93ca13d60f04763537d98ff9ef849680.png](93ca13d60f04763537d98ff9ef849680.png)

![e6d2ccd2f08e103c3a7db56fc6d09952.png](e6d2ccd2f08e103c3a7db56fc6d09952.png)

Index files are indentical in structure to the Heap files.

---

### The Query processing pipeline


![599bf404ff4d443eab9d95af692f3645.png](599bf404ff4d443eab9d95af692f3645.png)

- Parser: figuring out the meaning of the query, and validate that the query is valid. This step builds a Query Tree.
- Rewrite: Decompose views into underlying table references. This checks the Query Tree and modifies it to speeden things.
- Planner: Important piece: takes a look at Query Tree and come up with different plans to fetch that data.
- Execute: Run the plan given by the planner.

### EXPLAIN and EXPLAIN ANALYZE

EXPLAIN shows a query plan without executing it.
EXPLAIN ANALYZE shows a query plan, executes it and shows statistics about the execution.

![8e994816b13fbbf6a43e14ebf504d91f.png](8e994816b13fbbf6a43e14ebf504d91f.png)


![86856d7b3263d6a26e65be6677fa1482.png](86856d7b3263d6a26e65be6677fa1482.png)

![55dbc21aa70fb600c4e2a6f821898656.png](55dbc21aa70fb600c4e2a6f821898656.png)

![047891e43bc73c5ecf5a62e8fcd9b47c.png](047891e43bc73c5ecf5a62e8fcd9b47c.png)

The two numbers for the cost (8.31 .. 1756.11) are not a range.

The first number is the Cost for this step to produce the first row. **Startup Cost**

The second number is the Cost for this step to produce all rows. **Total Cost**

The reason is that in case of multiple processing steps, sometimes, as soon as the first row is processed, its result is passed to the next step (kinda like a stream).


![aa1c660c89a81bfbf1e2d64cd9117726.png](aa1c660c89a81bfbf1e2d64cd9117726.png)


Another point to note about costs in the second diagram below is that the costs flow up, i.e. a parent's cost is the sum of the cost of all its children + its own cost.

That is why the last HashJoin (blue) has a cost of 8.3, when in fact its own cost is close to zero. That cost is coming from the child Hash.


![a550d22edb49d498060ae6fa1b601895.png](a550d22edb49d498060ae6fa1b601895.png)

### How query cost is calculated 

![5912e4964af5804f64a21d85c3867fe2.png](5912e4964af5804f64a21d85c3867fe2.png)

More detailed: 

![5d594320df9de7bedd5dc49414b30ed4.png](5d594320df9de7bedd5dc49414b30ed4.png)

Official docs for each of these costs: https://www.postgresql.org/docs/9.5/runtime-config-query.html

These costs are all comapred to the baseline cost `seq_page_cost`

![66cd4924297b44b31d2d366f20053a35.png](66cd4924297b44b31d2d366f20053a35.png)

#### Quiz 

The formula for calculating the cost of a processing step in a query plan is: 

```
COST = (# pages read sequentially) * seq_page_cost
            + (# pages read at random) * random_page_cost
            + (# rows scanned) * cpu_tuple_cost 
            + (# index entries scanned) * cpu_index_tuple_cost 
            + (# times function/operator evalutated) * cpu_operator_cost

where 

seq_page_cost = 1.0 
random_page_cost = 4.0 
cpu_tuple_cost = 0.01 
cpu_index_tuple_cost = 0.005 
cpu_operator_cost = 0.0025
```

1. What is the cost for a query node that has to open 5 pages of data sequentially and then process 100 rows total? 

Answer: 5 x 1 + 100 x 0.01 = 6

2. What is the cost for a query node that has to open 4 pages of an index (probably at random), process 75 tuples from the index, then open 20 different pages from a heap file (also at random) and process 214 tuples?

Answer: 4 x 4 + 75 x 0.005 + 20 x 4 + 214 x 0.01 = 98.515