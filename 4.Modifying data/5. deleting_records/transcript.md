# Deleting Record

## Summary

### Select Before Delete Strategy

To prevent accidental data loss, always verify the data you intend to remove before executing a `DELETE` command. It is recommended to first run a `SELECT` query (e.g., `SELECT * FROM users WHERE ...` or `SELECT count(*) FROM users WHERE ...`) to ensure the `WHERE` clause targets exactly the records you expect. This precaution prevents catastrophic errors, such as wiping an entire table due to a typo.

### Executing Deletes

- **Basic Syntax:**
  ```sql
  DELETE FROM [table] WHERE [condition];
  ```
- **Returning Clause:** Similar to insert and update operations, you can append `RETURNING *` to the delete statement. This returns the data of the deleted rows, which is useful for application logic, logging, or archiving purposes.

### Alternative: Soft Deletes

Instead of permanently removing data, you can implement "soft deletes" (also known as tombstoning).

- **Method:** Add a column like `is_deleted` and update it to `true` instead of running a delete command.
- **Pros:** Data is preserved and can be recovered.
- **Cons:** The database table can become bloated over time. More importantly, every subsequent read query must include a filter (e.g., `WHERE is_deleted = false`) to prevent deleted records from appearing in the application.

### Best Practices

- Always maintain backups.
- Supabase and other tools may warn you before executing destructive operations without a `WHERE` clause.
- Complex `WHERE` clauses (e.g., deleting records older than 30 days) are supported, but they reinforce the need to "peek" at the data using a `SELECT` statement first.
