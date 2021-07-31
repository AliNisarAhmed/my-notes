# Materialized Views 

Query that gets executed only at very specific times, but the results are saved (cached) and can be referenced without re-running the query.

To create: 

```sql
CREATE MATERIALIZED VIEW <name_of_Mview> AS <query>
```

To Refresh: 

```sql 
REFRESH MATERIALIZED VIEW <name_of_MView>;
```

Advantage of MViews is that we can store the result of expensive queries (like monthly, weekly reports), that are needed often but do not change often, thus obviating the need to do the calculation every time.  
    - Later, a CRON job can be used to refreshed the MView whenever needed.

#### Example: 

![4a503cb32def3468d0bf60c8560c292c.png](4a503cb32def3468d0bf60c8560c292c.png)

The above query, which is slow, can be written as: 

```sql
SELECT 
    date_trunc('week', COALESCE(posts.created_at, comments.created_at)) AS week
    COUNT(posts.id) AS num_posts,
    COUNT(comments.id) AS num_comments
FROM likes 
LEFT JOIN posts ON posts.id = likes.post_id
LEFT JOIN comments ON comments.id = likes.comment_id
GROUP BY week
ORDER BY week
```

To create an MView, we can wrap the whole query above in: 

```sql
CREATE MATERIALIZED VIEW weekly_likes AS (...)

-- Next time we fetch the results, the query will run super fast

SELECT * FROM weekly_likes;

-- We can refresh the data like this: 

REFRESH MATERIALIZED VIEW weekly_likes;
```