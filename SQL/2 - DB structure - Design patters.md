
## DB Structure Design patterns

1. One way to implement a "Like" system - (a user can like multiple posts, a post can be liked by multiple users)

![1efcbcc87d171a5aa495b2106b3642c7.png](1efcbcc87d171a5aa495b2106b3642c7.png)

2. However, if we have more than one "Reaction" type (e.g. "Like", "Frown", "love", "care" etc.), then a technique called `polymorphic associations` is used.

The major problem with polymorphic associations is that we cannot use "foreign keys" for each entry in the polymorphic table, this data consistency becomes difficult.

![f2b801dc21df13e75677c8d8afb7ddce.png](f2b801dc21df13e75677c8d8afb7ddce.png)

3. Alternate way to implement `polymorphic associations`

![442145ce286279a25f82e7b7e12265d5.png](442145ce286279a25f82e7b7e12265d5.png)

We can now use foriegn keys.

The main problem here is that for each reaction type, we have to add a new column and initiate with null values.

Plus, the check/validation must be modified so that no more than 1 column has a non-null value at any given row.

4. Simplest alternative is to have an xref table for each reaction type.

![8ef99f86e191f8d272da07bd3a0e0c92.png](8ef99f86e191f8d272da07bd3a0e0c92.png)

The main drawback here is that the number of table increases, and querying becomes difficult.