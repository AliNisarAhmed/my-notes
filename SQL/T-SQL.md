# T-SQL

- The common belief that the term relational stems from relationships between tables is
  incorrect. “Relational” actually pertains to the mathematical term relation. In set theory, a
  relation is a representation of a set. In the relational model, a relation is a set of related
  information, with the counterpart in SQL being a table—albeit not an exact counterpart. A key
  point in the relational model is that a single relation should represent a single set (for
  example, Customers). Note that operations on relations (based on relational algebra) result in
  a relation (for example, a join between two relations).

- a relation is made of a heading and a body. The heading consists of a
  set of attributes (called columns in SQL), where each element is identified by an
  attribute name and a type name. The body consists of a set of tuples (called rows in
  SQL), where each element is identified by a key.

- SQL implements three-valued predicate logic by supporting the NULL marker to signify the generic concept of a missing value.

### Candidate Keys vs Foreign Keys

- Candidate Keys and Foreign keys are examples of **Constraints**.

- A candidate key is a key defined on one or more attributes that prevents more than one occurrence of the same tuple (row in SQL) in a relation.

  - A predicate based on a candidate key can uniquely identify a row (such as an
    employee).
  - You can define multiple candidate keys in a relation. For example, in an
    Employees relation, you can define candidate keys on employeeid, on SSN (Social Security
    number), and others.
  - Typically, you arbitrarily choose one of the candidate keys as the
    **_primary key_** (for example, employeeid in the Employees relation) and use that as the preferred
    way to identify a row. All other candidate keys are known as **_alternate keys_**.

- Foreign keys are used to enforce referential integrity. A foreign key is defined on one or
  more attributes of a relation (known as the referencing relation) and references a candidate
  key in another (or possibly the same) relation.
  - This constraint restricts the values in the
    referencing relation’s foreign-key attributes to the values that appear in the referenced
    relation’s candidate-key attributes.

### Normalization

- The relational model also defines normalization rules (also known as normal forms).

- Normalization is a formal mathematical process to guarantee that each entity will be
  represented by a single relation.

- In a normalized database, you avoid anomalies during data
  modification and keep redundancy to a minimum without sacrificing completeness.

The following are the first three normal forms (1NF, 2NF, and 3NF) introduced by Codd.

#### 1NF

The first normal form says that the tuples (rows) in the relation (table) must be unique and
attributes should be atomic. This is a redundant definition of a relation; in other words, if a
table truly represents a relation, it is already in first normal form.

You achieve unique rows in SQL by defining a unique key for the table.

Atomicity of attributes is subjective in the same way that the definition of a set is
subjective. As an example, should an employee name in an Employees relation be expressed
with one attribute (fullname), two (firstname and lastname), or three (firstname, middlename,
and lastname)? The answer depends on the application. If the application needs to manipulate
the parts of the employee’s name separately (such as for search purposes), it makes sense to
break them apart; otherwise, it doesn’t.

#### 2NF

The second normal form involves two rules.

- One rule is that the data must meet the first normal form.
- The other rule addresses the relationship between nonkey and candidate-key attributes.

For every candidate key, every nonkey attribute has to be fully functionally
dependent on the entire candidate key.

In other words, a nonkey attribute cannot be fully functionally dependent on part of a candidate key.

To put it more informally, if you need to obtain any nonkey attribute value, you need to provide the values of all attributes of a candidate key from the same tuple. You can find any value of any attribute of any tuple if you
know all the attribute values of a candidate key.

![2NF Example](../images/sql-1.PNG)

![2NF Example](../images/sql-2.PNG)

#### 3NF

The third normal form also has two rules.

- The data must meet the second normal form.
- Also, all nonkey attributes must be dependent on candidate keys nontransitively.

Informally, this rule means that all nonkey attributes must be mutually independent. In other words, one nonkey
attribute cannot be dependent on another nonkey attribute.

![2NF Example](../images/3nf.PNG)

### CHECK Constraint

You can use a check constraint to define a predicate that a row must meet to be entered into the
table or to be modified.

```sql
ALTER TABLE dbo.Employees
ADD CONSTRAINT CHK_Employees_salary
CHECK(salary > 0.00);
```

After the above, salary -5000 will be rejected, while salary 50,000 and NULL will both be excepted (if the column allows NULLs that is).

### DEFAULT Constraint

A default constraint is associated with a particular attribute. It’s an expression that is used as
the default value when an explicit value is not specified for the attribute when you insert a row.

```sql
ALTER TABLE dbo.Orders
ADD CONSTRAINT DFT_Orders_orderts
DEFAULT(SYSDATETIME()) FOR orderts;
```

---

---

---

### Logical Query Processing

which simply means how the sql query is actually performed by the DB engine (as opposed to how it is actually written)

Sample Query

```sql
USE TSQLV4;

SELECT empid, YEAR(orderdate) AS orderyear, COUNT(*) AS numorders
FROM Sales.Orders
WHERE custid = 71
GROUP BY empid, YEAR(orderdate)
HAVING COUNT(*) > 1
ORDER BY empid, orderyear;
```

- The code starts with a USE statement that ensures that the database context of your session
  is the TSQLV4 sample database. If your session is already in the context of the database you
  need to query, the USE statement is not required.

- The clauses are logically processed in the following order
  1. FROM
  2. WHERE
  3. GROUP BY
  4. HAVING
  5. SELECT
  6. ORDER BY

#### FROM Clause

The FROM clause is the very first query clause that is logically processed. In this clause, you
specify the names of the tables you want to query and table operators that operate on those
tables.

#### WHERE Clause

In the WHERE clause, you specify a predicate or logical expression to filter the rows returned
by the FROM phase.

Only rows for which the logical expression evaluates to TRUE are returned by the WHERE phase to the subsequent logical query processing phase.

The WHERE phase returns rows for which the logical expression evaluates to TRUE, and it doesn’t return rows for which the logical expression evaluates to FALSE or UNKNOWN.

#### GROUP BY Clause

```sql
FROM Sales.Orders
WHERE custid = 71
GROUP BY empid, YEAR(orderdate)
```

This means that the GROUP BY phase produces a group for each **unique combination** of employee-ID and order-year values that appears in the data returned by the WHERE phase.

If the query involves grouping, all phases subsequent to the GROUP BY phase—including HAVING, SELECT, and ORDER BY—must operate on groups as opposed to operating on individual rows.

Each group is ultimately represented by a single row in the final result of the query. This implies that all expressions you specify in clauses that are processed in phases subsequent to the GROUP BY phase are required to guarantee returning a scalar (single value) per group.

Expressions based on elements that participate in the GROUP BY clause meet the requirement because, by definition, each group has only one unique occurrence of each GROUP BY element.

For example, in the group for employee ID 8 and order year 2015, there’s only one unique employee-ID value and only one unique order-year value. Therefore, you’re allowed to refer to the expressions empid and YEAR(orderdate) in clauses that are processed in phases subsequent to the GROUP BY phase, such as the SELECT clause.

Elements that do not participate in the GROUP BY clause are allowed only as inputs to an aggregate function such as COUNT, SUM, AVG, MIN, or MAX.

**Note**: that all aggregate functions ignore NULLs, with one exception—COUNT(\*). For example, consider a group of five rows with the values 30, 10, NULL, 10, 10 in a column called qty. The expression COUNT(\*) returns 5 because there are five rows in the group, whereas COUNT(qty) returns 4 because there are four known values. If you want to handle
only distinct occurrences of known values, specify the DISTINCT keyword before the input
expression to the aggregate function.

#### HAVING clause

Whereas the WHERE clause is a row filter, the HAVING clause is a group filter. Only groups
for which the HAVING predicate evaluates to TRUE are returned by the HAVING phase to the
next logical query processing phase. Groups for which the predicate evaluates to FALSE or
UNKNOWN are discarded.

#### SELECT clause

The SELECT clause is where you specify the attributes (columns) you want to return in the
result table of the query. You can base the expressions in the SELECT list on attributes from
the queried tables, with or without further manipulation.

Remember that the SELECT clause is processed after the FROM, WHERE, GROUP BY, and
HAVING clauses. This means that aliases assigned to expressions in the SELECT clause do not
exist as far as clauses that are processed before the SELECT clause are concerned.

It’s a typical mistake to try and refer to expression aliases in clauses that are processed before the
SELECT clause.

Curiously, you are not allowed to refer to column aliases created in the SELECT clause in
other expressions within the same SELECT clause. That’s the case even if the expression that
tries to use the alias appears to the right of the expression that created it. For example, below is invalid

```sql
SELECT orderid,
YEAR(orderdate) AS orderyear,
orderyear + 1 AS nextyear
FROM Sales.Orders;
```

#### ORDER BY clause

You use the ORDER BY clause to sort the rows in the output for presentation purposes. In
terms of logical query processing, ORDER BY is the very last clause to be processed.

**NOTE:**

One of the most important points to understand about SQL is that a table — be it an existing
table in the database or a table result returned by a query—has no guaranteed order. That’s
because a table is supposed to represent a set of rows (or multiset, if it has duplicates), and a
set has no order. This means that when you query a table without specifying an ORDER BY
clause, SQL Server is free to return the rows in the output in any order. The only way for you
to guarantee the presentation order in the result is with an ORDER BY clause. However, you
should realize that if you do specify an ORDER BY clause, the result can’t qualify as a table
because it is ordered. Standard SQL calls such a result a **cursor**.

The ORDER BY phase is the only phase in which you can refer to column aliases created in the SELECT phase, because it is the only phase processed after the SELECT phase.

Note that if you define a column alias that is the same as an underlying column name, as in 1 - col1 AS col1, and refer to that alias in the ORDER BY clause, the new column is the one considered for ordering.

With T-SQL, you also can specify elements in the ORDER BY clause that do not appear in
the SELECT clause, meaning you can sort by something you don’t necessarily want to return.
The big drawback for this is that you can’t check your sorted results by looking at the query
output.

---

### TOP and OFFSET-FETCH filters

The TOP filter is a proprietary T-SQL feature you can use to limit the number or percentage
of rows your query returns. It relies on two elements as part of its specification: one is the
number or percent of rows to return, and the other is the ordering.

Note that when the TOP filter is specified, the ORDER BY clause serves a dual purpose
in the query. One purpose is to define the presentation ordering for the rows in the query
result. Another purpose is to define for the TOP option which rows to filter.

You can use the TOP option with the PERCENT keyword, in which case SQL Server
calculates the number of rows to return based on a percentage of the number of qualifying
rows, rounded up.

Note that you can even use the TOP filter in a query without an ORDER BY clause. In such a
case, the ordering is completely undefined—SQL Server returns whichever n rows it happens
to physically access first, where n is the requested number of rows.

With the OFFSET clause you indicate how many rows to skip, and with the FETCH clause you indicate
how many rows to filter after the skipped rows.

```sql
SELECT orderid, orderdate, custid, empid
FROM Sales.Orders
ORDER BY orderdate, orderid
OFFSET 50 ROWS FETCH NEXT 25 ROWS ONLY;
```

Note that a query that uses OFFSET-FETCH must have an ORDER BY clause.

Also, T-SQL doesn’t support the FETCH clause without the OFFSET clause. If you do not want to skip any
rows but do want to filter rows with the FETCH clause, you must indicate that by using
OFFSET 0 ROWS.

However, OFFSET without FETCH is allowed. In such a case, the query
skips the indicated number of rows and returns all remaining rows in the result.

The singular and plural forms ROW and ROWS are interchangeable. Therefore, you’re allowed to use
the form FETCH 1 ROW. The same principle applies to the OFFSET clause. Also, if you’re
not skipping any rows (OFFSET 0 ROWS), you might find the term “first” more suitable than
“next.” Hence, the forms FIRST and NEXT are interchangeable.

---

### Use of `N` before strings

In the code snippet below:

```sql
SELECT empid, firstname, lastname
FROM HR.Employees
WHERE lastname LIKE N'D%';
```

Notice the use of the letter N to prefix the string ‘D%’; it stands for National and is used to
denote that a character string is of a Unicode data type (NCHAR or NVARCHAR), as opposed
to a regular character data type (CHAR or VARCHAR). Because the data type of the lastname
attribute is NVARCHAR(40), the letter N is used to prefix the string.

---

### Case Expressions

A CASE expression is a scalar expression that returns a value based on conditional logic. It is
based on the SQL standard. Note that CASE is an expression and not a statement; that is, it
doesn’t take action such as controlling the flow of your code. Instead, it returns a value.
Because CASE is a scalar expression, it is allowed wherever scalar expressions are allowed,
such as in the SELECT, WHERE, HAVING, and ORDER BY clauses and in CHECK constraints.

#### Simple vs Searched

There are two forms of CASE expressions: simple and searched. You use the simple form to
compare one value or scalar expression with a list of possible values and return a value for
the first match. If no value in the list is equal to the tested value, the CASE expression returns
the value that appears in the ELSE clause (if one exists). If the CASE expression doesn’t have
an ELSE clause, it defaults to ELSE NULL.

```sql
SELECT productid, productname, categoryid,
  CASE categoryid
    WHEN 1 THEN 'Beverages'
    WHEN 2 THEN 'Condiments'
    WHEN 3 THEN 'Confections'
    WHEN 4 THEN 'Dairy Products'
    WHEN 5 THEN 'Grains/Cereals'
    WHEN 6 THEN 'Meat/Poultry'
    WHEN 7 THEN 'Produce'
    WHEN 8 THEN 'Seafood'
    ELSE 'Unknown Category'
  END AS categoryname
FROM Production.Products;
```

The searched CASE form is more flexible in the sense you can specify predicates in the WHEN clauses rather than being
restricted to using equality comparisons. The searched CASE expression returns the value in
the THEN clause that is associated with the first WHEN predicate that evaluates to TRUE. If
none of the WHEN predicates evaluates to TRUE, the CASE expression returns the value that
appears in the ELSE clause (or NULL if an ELSE clause is not present).

```sql
SELECT orderid, custid, val,
  CASE
    WHEN val < 1000.00 THEN 'Less than 1000'
    WHEN val BETWEEN 1000.00 AND 3000.00 THEN 'Between 1000 and 3000'
    WHEN val > 3000.00 THEN 'More than 3000'
    ELSE 'Unknown'
  END AS valuecategory
FROM Sales.OrderValues;
```

#### Some functions related to CASE

T-SQL supports some functions you can consider as abbreviations of the CASE expression:

##### ISNULL

The ISNULL function accepts two arguments as input and returns the first that is not NULL,
or NULL if both are NULL. For example ISNULL(col1, ‘’) returns the col1 value if it isn’t
NULL and an empty string if it is NULL.

##### COALESCE

The COALESCE function is similar, only it supports
two or more arguments and returns the first that isn’t NULL, or NULL if all are NULL.

#### IIF

is like a ternary operator in JS

#### CHOOSE

Choose from one of the values based on the index given in first argument.
The expression CHOOSE(3, col1, col2, col3) returns the value of col3.

---

### NULLs

SQL supports the NULL marker to represent missing values and uses three-valued predicate logic,
meaning that predicates can evaluate to TRUE, FALSE, or UNKNOWN.

SQL has different treatments for UNKNOWN in different language elements (and for some
people, not necessarily the expected treatments). The treatment SQL has for query filters is
“accept TRUE,” meaning that both FALSE and UNKNOWN are discarded. Conversely, the
definition of the treatment SQL has for CHECK constraints is “reject FALSE,” meaning that
both TRUE and UNKNOWN are accepted.

One of the tricky aspects of the logical value UNKNOWN is that when you negate it, you
still get UNKNOWN. For example, given the predicate NOT (salary > 0), when salary is
NULL, salary > 0 evaluates to UNKNOWN, and NOT UNKNOWN remains UNKNOWN.

What some people find surprising is that an expression comparing two NULLs (NULL =
NULL) evaluates to UNKNOWN. The reasoning for this from SQL’s perspective is that a
NULL represents a missing value, and you can’t really tell whether one missing value is equal
to another. Therefore, SQL provides you with the predicates IS NULL and IS NOT NULL,
which you should use instead of = NULL and <> NULL.

SQL also treats NULLs inconsistently in different language elements for comparison and
sorting purposes. Some elements treat two NULLs as equal to each other, and others treat them
as different.

For example, for grouping and sorting purposes, two NULLs are considered equal. That is,
the GROUP BY clause arranges all NULLs in one group just like present values, and the
ORDER BY clause sorts all NULLs together. Standard SQL leaves it to the product
implementation to determine whether NULLs sort before present values or after them, but it
must be consistent within the implementation. T-SQL sorts NULLs before present values.

For the purposes of enforcing a UNIQUE constraint, standard SQL treats NULLs as
different from each other (allowing multiple NULLs). Conversely, in T-SQL, a UNIQUE
constraint considers two NULLs as equal (allowing only one NULL).

---

### All-at-once Operations

SQL supports a concept called all-at-once operations, which means that all expressions that
appear in the same logical query processing phase are evaluated logically at the same point in
time. The reason for this is that all expressions that appear in the same logical phase are
treated as a set, and as mentioned earlier, a set has no order to its elements.

This concept explains why, for example, you cannot refer to column aliases assigned in the
SELECT clause within the same SELECT clause.

```sql
-- This will result in an error
SELECT
  orderid,
  YEAR(orderdate) AS orderyear,
  orderyear + 1 AS nextyear   -- orderyear does not exist as SELECT is all-at-once
FROM Sales.Orders;
```

Similarly, WHERE is also all-at-once

```sql
SELECT col1, col2
FROM dbo.T1
WHERE col1 <> 0 AND col2/col1 > 2;
```

The Not-equal-to-zero check above is not guaranteed (SQL may evaluate the second expression first)

One way to avoid the above is to use CASE-WHEN (the order in which the WHEN
clauses of a CASE expression are evaluated is guaranteed.)

---

### Character Data Types

SQL Server supports two kinds of character data types: regular and Unicode. Regular data
types include CHAR and VARCHAR, and Unicode data types include NCHAR and NVARCHAR.

The two kinds of character data types also differ in the way in which literals are expressed.
When expressing a regular character literal, you simply use single quotes: ‘This is a regular
character string literal’. When expressing a Unicode character literal, you need to specify the
character N (for National) as a prefix: N’This is a Unicode character string literal’.

Any data type without the VAR element (CHAR, NCHAR) in its name has a fixed length,
which means that SQL Server preserves space in the row based on the column’s defined size
and not on the actual number of characters in the character string. For example, when a
column is defined as CHAR(25), SQL Server preserves space for 25 characters in the row
regardless of the length of the stored character string.

A data type with the VAR element (VARCHAR, NVARCHAR) in its name has a variable
length, which means that SQL Server uses as much storage space in the row as required to
store the characters that appear in the character string, plus two extra bytes for offset data. For
example, when a column is defined as VARCHAR(25), the maximum number of characters
supported is 25, but in practice, the actual number of characters in the string determines the
amount of storage.

You can also define the variable-length data types with the MAX specifier instead of a
maximum number of characters. When the column is defined with the MAX specifier, any
value with a size up to a certain threshold (8,000 bytes by default) can be stored inline in the
row (as long as it can fit in the row). Any value with a size above the threshold is stored
external to the row as a large object (LOB).

#### Operators and Functions

##### `+` and `CONCAT` function

`+` concatenates strings, but manual NULL replacement is required

`CONCAT`: accepts a list of inputs for concatenation and automatically substitutes NULLs with empty strings.

##### The `SUBSTRING` function

The SUBSTRING function extracts a substring from a string.

`SUBSTRING(string, start, length)`

If the value of the third argument exceeds the end of the input string, the function returns
everything until the end without raising an error.

##### The LEFT and RIGHT functions

The LEFT and RIGHT functions are abbreviations of the SUBSTRING function, returning a
requested number of characters from the left or right end of the input string.

`LEFT(string, n), RIGHT(string, n)`

The first argument, string, is the string the function operates on. The second argument, n, is
the number of characters to extract from the left or right end of the string.

##### The LEN and DATALENGTH functions

The LEN function returns the number of characters in the input string.

To get the number of bytes, use the DATALENGTH function instead of LEN.

##### The CHARINDEX function

The CHARINDEX function returns the position of the first occurrence of a substring within a
string.

`CHARINDEX(substring, string[, start_pos])`

##### The PATINDEX function

The PATINDEX function returns the position of the first occurrence of a pattern within a
string.

`PATINDEX(pattern, string)`

the following example shows how to find the position of the first occurrence of a digit
within a string:

```sql
SELECT PATINDEX('%[0-9]%', 'abcd123efgh');
```

This code returns the output 5. (The index of number `1`, index count starts from 1 in SQL)

##### The REPLACE function

The REPLACE function replaces all occurrences of a substring with another.

`REPLACE(string, substring1, substring2)`

The function replaces all occurrences of substring1 in string with substring2.

##### The REPLICATE function

The REPLICATE function replicates a string a requested number of times.

`REPLICATE(string, n)`

##### The STUFF function

You use the STUFF function to remove a substring from a string and insert a new substring
instead.

`STUFF(string, pos, delete_length, insert_string)`

This function operates on the input parameter string. It deletes as many characters as the
number specified in the delete_length parameter, starting at the character position specified in
the pos input parameter. The function inserts the string specified in the insert_string parameter
in position pos.

##### The UPPER and LOWER functions

The UPPER and LOWER functions return the input string with all uppercase or lowercase
characters, respectively.

`UPPER(string), LOWER(string)`

##### The RTRIM and LTRIM functions

The RTRIM and LTRIM functions return the input string with leading or trailing spaces
removed.

`RTRIM(string), LTRIM(string)`

If you want to remove both leading and trailing spaces, use the result of one function as the
input to the other.

##### The STRING_SPLIT function

The STRING_SPLIT table function splits an input string with a separated list of values into the
individual elements. This function was introduced in SQL Server 2016.

`SELECT value FROM STRING_SPLIT(string, separator);`
