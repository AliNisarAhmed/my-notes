# Prepared Statements 

https://www.postgresql.org/docs/current/sql-prepare.html

A prepared statement is a server-side object that can be used to optimize performance. When the `PREPARE` statement is executed, the specified statement is parsed, analyzed, and rewritten. When an `EXECUTE` command is subsequently issued, the prepared statement is planned and executed. This division of labor avoids repetitive parse analysis work, while allowing the execution plan to depend on the specific parameter values supplied.

`PREPARE` statements are also used to sanitize the input to avoid SQL injection attacks.