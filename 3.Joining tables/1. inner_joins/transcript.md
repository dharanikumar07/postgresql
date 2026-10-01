# Inner joins

## Summary

### Introduction to Data Relationships

- Real-world data is typically spread across multiple tables rather than confined to a single one.
- To analyze this distributed data effectively, tables must be connected using joins.
- The relationship between tables is established using specific keys:
  - **Primary Key (PK):** A unique identifier for a record within its own table (e.g., `id` in a `users` table).
  - **Foreign Key (FK):** A field in one table that refers to the Primary Key of another table (e.g., `user_id` in a `posts` table).
  - **Naming Convention:** It is standard practice to name the primary key `id` and the foreign key as the singular table name suffixed with `_id` (e.g., `user_id`).

### Understanding Inner Joins

- **Purpose:** An `INNER JOIN` connects two tables and returns only the rows where there is a match in both tables.
- **Venn Diagram Visualization:** If one circle represents `users` and another represents `posts`, an inner join returns only the overlapping section in the middle. It excludes:
  - Users who have made no posts.
  - "Orphaned" posts that have no associated user.
- **Default Behavior:** The `INNER JOIN` is the default join type in SQL. Writing just `JOIN` functions the same as `INNER JOIN`, though being explicit is often preferred for clarity.

### SQL Syntax and Best Practices

Basic syntax:

```sql
SELECT columns
FROM table_A
INNER JOIN table_B
  ON table_A.foreign_key = table_B.primary_key;
```

- **Ambiguity:** PostgreSQL cannot automatically determine how tables relate; you must explicitly define the link using the `ON` clause (e.g., `ON posts.user_id = users.id`).
- **Column Prefixes:** When selecting columns that exist in multiple tables (like `id` or `created_at`), you must prefix the column name with the table name to avoid "ambiguous column" errors (e.g., `SELECT users.name, posts.title`).
- **Aliasing:** To save time typing long table names, you can use aliases:
  ```sql
  FROM users u
  JOIN posts p ON p.user_id = u.id
  ```
- **Filtering:** All standard filtering techniques (like `WHERE` clauses) can be applied to joined data to further refine the results (e.g., filtering for posts created in the last 30 days).
