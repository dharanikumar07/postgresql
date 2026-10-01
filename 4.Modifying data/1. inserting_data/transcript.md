# Inserting data

## Summary

This video introduces CRUD operations beyond reading data, focusing specifically on inserting data into PostgreSQL and leveraging its unique features to optimize performance and code simplicity.

### Key Takeaways

- **Basic Insert Syntax:**
  ```sql
  INSERT INTO table (columns) VALUES (values);
  ```

### Network Optimization (Round Trips)

- Every interaction with the database incurs a "round trip" penalty (network latency), which increases if the application and database are geographically distant.
- Ideally, the application and database should be located as close as possible (e.g., the same building/availability zone).

### Bulk Inserts

- Instead of executing multiple separate `INSERT` statements for multiple rows (which creates multiple network round trips), you should combine them into a single statement.
  ```sql
  INSERT INTO table (columns) VALUES (row1), (row2), (row3);
  ```
- This method sends one request over the wire and receives one response, significantly improving efficiency.
- **Handling Default Values:** PostgreSQL automatically handles columns defined with defaults (like auto-incrementing IDs or timestamps like `created_at`) so developers only need to insert the specific data required (e.g., `name` and `email`).

### The RETURNING Clause

- Unlike many other databases, PostgreSQL allows you to immediately retrieve data from the row you just inserted without running a second `SELECT` query.
- Syntax: append `RETURNING *` (for all columns) or `RETURNING id, email` (for specific columns) to the end of the insert statement.
- This feature further reduces network round trips by combining the insert and the retrieval of the new ID into a single operation.
