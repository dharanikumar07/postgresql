# Updating data

## Summary

### The Danger of Unfiltered Updates

Running an `UPDATE` command without a `WHERE` clause is a critical mistake that will modify every single record in a database. Even if a user intends to filter by a specific ID, writing the update portion first creates a "dangling update" risk. If executed prematurely by accident, this can disastrously overwrite massive amounts of data (e.g., changing every user's email to the same value).

### The Golden Rule: Select, Then Update

To prevent data disasters, you should adopt a specific workflow for every update, regardless of how simple it seems:

1. **Write a SELECT statement.** First, write a `SELECT` query using the exact `WHERE` clause you intend to use for the modification. This allows you to verify that you are targeting the correct specific row (or set of rows).
2. **Verify the results.** Run the `SELECT` query to ensure the data returned matches your expectations. If a query meant to update one record returns a million rows, you know your logic is flawed before causing damage.
3. **Convert to UPDATE.** Only after verification should you replace the `SELECT` portion with the `UPDATE` command, keeping the vetted `WHERE` clause intact.

### Additional Tips

- **Complex Logic:** This habit is particularly vital when dealing with complex conditions involving dates, intervals, or multiple booleans, where logic errors are more common.
- **Returning Clause:** PostgreSQL supports the `RETURNING *` clause at the end of an update statement, allowing you to immediately see the changes made to the modified records.
- **Consistency:** Apply this method even to "easy" updates, as complacency is often the cause of severe database errors.
