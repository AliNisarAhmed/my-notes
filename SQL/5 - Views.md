# Views

Views are pseudo-tables. 

When a view is created, postgres stores that query, and then the View can later be referred to as a Table in postgres. 

When we retrieve an item from a View, the query/code inside the View is executed afresh.

- A view can represent a subset of a real table, selecting certain columns or certain rows from an ordinary table.
- A view can even represent joined tables. 

Because views are assigned separate permissions, you can use them to restrict table access so that the users see only specific rows or columns of a table.


```sql
CREATE [TEMP | TEMPORARY] VIEW view_name AS
SELECT column1, column2..... FROM table_name
WHERE [condition]; 
```

Example: 

Lets say we have the following query, fetching data from `users` table after joining it with the union of `photo_tags` and `caption_tags` tables. 

(The query fetches users who have been tagged the most in posts and photos).

```sql
SELECT username, COUNT(*)
FROM users
JOIN (
    SELECT user_id FROM photo_tags
    UNION ALL 
    SELECT user_id FROM caption_tags
) AS tags ON tags.user_id = users.id 
GROUP BY username 
ORDER BY COUNT(*) DESC;
```

Instead of writing the inner join again and again, we can create a View to combine all kinds of tags inside a pseudo-table, instead of actually creating a unified tags table.

```sql
CREATE VIEW tags AS (
    SELECT id, created_at, user_id, post_id, 'photo_tag' AS type FROM photo_tags
    UNION ALL 
    SELECT id, created_at, user_id, post_id, 'caption_tag' AS type FROM caption_tags
);

-- Now the previous query can be simplified as

SELECT username 
FROM users 
JOIN tags on tags.user_id = users.id 
GROUP BY username 
ORDER BY COUNT(*) DESC;
```

---

### Updating and Deleting a View

Since a View query is executed every time a View is used, there is no need to "refresh" a View. 

However, to update the underlying querr of a View, we can do: 

```sql
CREATE OR REPLACE VIEW recent_posts AS (..)
```


To delete a View, use

```sql
DROP VIEW view_name;
```