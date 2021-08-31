# Migrations 

Migrations solve the following issues:

1. Changes to the DB structure and changes to clients need to be made at precisely the same time.
2. When working with other engineers, we need a really easy way to tie the structure of our DB to our code.

Migrations can be of two types: 
1. Schema Migrations 
2. Data Migrations

There are several reasons NOT to run Data migrations at the same time as Schema migrations. (One of the reasons discussed below)

Consider the example below: 

![09d630ccf453877bfaa42daee1a2b4e0.png](09d630ccf453877bfaa42daee1a2b4e0.png)

The problem is running a single migration with lot of data takes time, and in this time, new data may come in, leaving the data in a bad shape.

(The migration below is run in a Transaction)

![0c6131ca82cd5ebcba828e44b7024b24.png](0c6131ca82cd5ebcba828e44b7024b24.png)

![1cf5fd6e3a5b1847775db2e494acbe9a.png](1cf5fd6e3a5b1847775db2e494acbe9a.png)

For the above migration, the data is copied at the time the migration is first created, and by the time the migration is done, that data may already be outdated.

Correct way to carry out the above migration is: 

![fa4923ea78237c135b82862373888d9b.png](fa4923ea78237c135b82862373888d9b.png)

Further explanation of the above process: 

![8b47a06f9c05c0615fdb9a940171f2d6.png](8b47a06f9c05c0615fdb9a940171f2d6.png)
