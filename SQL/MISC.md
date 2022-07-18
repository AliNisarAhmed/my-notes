# Miscellaneous topics 

### COUNT DISTINCT

```sql
-- counting the distinct values in the passed column.

select count(distinct memid)  from cd.bookings 

-- Equivalent to below:

select count(*)  from  (select  distinct memid from cd.bookings)  as mems
```

### TO_CHAR function

https://www.postgresqltutorial.com/postgresql-to_char/

```sql
SELECT fcs.facid, fcs.name, TRIM(TO_CHAR(SUM(bks.slots) / 2.0, 'FM9999999D00')) AS "Total Hours"
FROM cd.facilities AS fcs 
INNER JOIN cd.bookings AS bks ON bks.facid = fcs.facid 
GROUP BY fcs.facid, fcs.name
ORDER BY fcs.facid;
```

### COALESCE function 

The syntax of the `COALESCE` function is as follows:

`COALESCE (argument_1, argument_2, …);`


The `COALESCE` function accepts an unlimited number of arguments. It returns the first argument that is not null. If all arguments are null, the `COALESCE` function will return null.

The `COALESCE` function evaluates arguments from left to right until it finds the first non-null argument. All the remaining arguments from the first non-null argument are not evaluated.