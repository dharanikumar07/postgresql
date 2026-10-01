# Returning data

## Summary

Based on the video transcript, here is a summary of the key points regarding the `RETURNING` keyword and new features in Postgres 18:

- **The Power of Layering Concepts:** The video emphasizes building expertise by layering new database concepts on top of foundational ones, specifically combining the `RETURNING` clause with "pseudo tables."

### New Pseudo Tables in Postgres 18

- Alongside the existing `EXCLUDED` pseudo table (used in upserts), Postgres 18 introduces `NEW` and `OLD`.
- These tables allow access to data values immediately before and after a modification during a single query execution.

### Using NEW and OLD with RETURNING

- When performing an `UPDATE`, you can use `RETURNING new.column, old.column` to retrieve both the updated value and the previous value in the same response.
- **Use Cases:**
  - **Audit Logging:** Tracking changes without needing separate read queries.
  - **UI Feedback:** Displaying "Changed X to Y" messages to users immediately after an update.
  - **Efficiency:** Eliminates the need for the application to fetch the old data before issuing the update, reducing code complexity.

### Archiving Data via CTEs (Common Table Expressions)

The video demonstrates a powerful pattern using `RETURNING` within a CTE to move data between tables (e.g., from a "current" table to an "archive" table) in a single atomic operation.

The process:

1. Define a CTE that deletes a row from the primary table.
2. Use `RETURNING` within the delete statement to capture the deleted data.
3. Immediately `INSERT` that captured data into the archive table.

**Benefit:** This efficient method keeps active tables small and fast while preserving history, all without requiring multiple round-trips between the application and the database.
