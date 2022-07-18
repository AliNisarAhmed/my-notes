# Window Functions 

Similar to an aggregate function, a window function operates on a set of rows. However, it does not reduce the number of rows returned by the query.

The term _window_ describes the set of rows on which the window function operates. A window function returns values from the rows in a window.



https://pgexercises.com/questions/aggregates/countmembers.html

https://www.postgresqltutorial.com/postgresql-window-function/


Window functions operate on the result set of your (sub-)query, after the WHERE clause and all standard aggregation. They operate on a _window_ of data. By default this is unrestricted: the entire result set, but it can be restricted to provide more useful results

```sql
-- Both statements are equivalent

select  (select count(*)  from cd.members)  as count, firstname, surname from cd.members order  by joindate

--

select count(*)  over(), firstname, surname from cd.members order  by joindate

```

For example, suppose instead of wanting the count of all members (like above), we want the count of all members who joined in the same month as that member:

```sql
select 
    count(*) over(partition by date_trunc('month',joindate)), 
    firstname, 
    surname 
from cd.members 
order  by joindate
```

In this example, we partition the data by month. For each row the window function operates over, the window is any rows that have a joindate in the same month. The window function thus produces a count of the number of members who joined in that month.