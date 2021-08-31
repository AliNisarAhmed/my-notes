# Schema

A schema is like a sub-folder within a Database.

Schema contains tables, data types, functions and operators. 

Unlike databases, schemas are not rigidly separated: a user can access objects in any of the schemas in the database they are connected to, if they have privileges to do so.

A schema can be created like this: 

`CREATE SCHEMA myschema;`

and then tables in that schema can be accessed like this: 

`schema.table`

and then deleted like this: 

`DROP SCHEMA myschema;`

use CASCASE to delete all the data inside as well:

`DROP SCHEMA mySchema CASCADE;`

### Search Paths

If a schema prefix is not specified when accessing a table, Postgres uses `schema_path` to clarify which schema to access first. 

That default schema order is defined in the variable `search_path`

By default, `search_path` is `"$user", public` ("$user" means if a schema with the same name as logged in use exists, it is accessed first)

We can alter search paths: 

`SET search_path TO myschema,public;`


### Schemas and previleges

By default, users cannot access any objects in schemas they do not own. To allow that, the owner of the schema must grant the `USAGE` privilege on the schema. To allow users to make use of the objects in the schema, additional privileges might need to be granted, as appropriate for the object.

More here: https://www.postgresql.org/docs/current/ddl-schemas.html


### User Authorization

We can create a schema owned by a user like this: 

`CREATE SCHEMA schema_name AUTHORIZATION user_name;`

If the schema_name is omitted, the new schema will have the same name as the user.