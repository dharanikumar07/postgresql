# Understanding the vocabulary

## Summary

Here is a summary of the video transcript regarding PostgreSQL's organizational structure:

- **The Instance:** This is the highest level of organization, representing the running server or cluster (the actual PostgreSQL software running on a VM or machine).

### Databases

- An instance contains one or more databases.
- When connecting to an instance, a specific database must be selected (often part of the connection string).
- Database names are customizable, though services like Supabase default to a database named `postgres`.
- Databases within an instance are isolated and do not talk to one another.

### Schemas

- This is an intermediate layer unique to PostgreSQL (compared to MySQL or SQLite) that sits between the database and the tables.
- Schemas act as namespaces or folders to organize tables.
- **The Public Schema:** By default, PostgreSQL provides a `public` schema where all tables are created unless specified otherwise. This is sufficient for most use cases.
- **Use Cases:** Schemas are useful for organization (e.g., separating admin, app, and reporting tables), multi-tenancy, or isolating extensions.
- **Terminology Note:** The word "schema" is overloaded. It can refer to this namespace folder, but also to the structure/definition of a specific table (columns, types, constraints).

### The Search Path

- This setting determines the order in which PostgreSQL looks for tables in different schemas when a query is run without a specific prefix.
- The default path usually looks in a user-specific schema first, then the public schema, and finally an extensions schema.
- You can explicitly target a table in a specific schema by prepending the schema name:

```sql
SELECT * FROM extensions.users;
```

### Tables

- These are the bottom layer where the actual data resides, living inside a specific schema.
