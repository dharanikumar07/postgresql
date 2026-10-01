# Generated columns from JSON

## Summary

This video explains how to use generated columns in PostgreSQL to fix data modeling issues, specifically when important data is buried inside a JSON blob and needs to be accessed as a top-level column.

### The Problem

- Data is often stored in unstructured JSON blobs (e.g., a `metadata` column).
- Business requirements may change, making a field within that JSON (like `price`) critical for filtering, selecting, or indexing.
- Standard JSON columns are harder to constrain and index efficiently compared to structured columns.

### The Solution: Generated Columns

- **Concept:** A generated column functions like a formula in a spreadsheet (e.g., `A1 + B1`). The database takes full responsibility for calculating the value based on other columns in the same row.
- **Automation:** The user does not insert data into this column manually; PostgreSQL automatically calculates and populates it.

### Implementation Steps

To promote a field from a JSON blob to a structured column, you use the `ALTER TABLE` command with specific keywords:

- **Extraction Logic:** Define the extraction and casting logic (e.g., taking the price from `metadata` and casting it to an integer).

```sql
ALTER TABLE products
ADD COLUMN price int
GENERATED ALWAYS AS (cast(metadata->>'price' as int)) STORED;
```

- **Keywords:**
  - `GENERATED ALWAYS AS`: Tells Postgres to manage the column content using the provided formula.
  - `STORED`: Instructs the database to calculate the value and physically write it to the disk (persisted), rather than calculating it on the fly during reads.

### Key Benefits and Behaviors

- **Automatic Synchronization:** When the source data (the JSON blob) is updated via an `UPDATE` statement, the generated column automatically updates to reflect the change.
- **Read-Only:** Users cannot manually `UPDATE` or `SET` the value of a generated column; an error will occur if attempted.
- **Application Code:** No changes are required on the application side. The app continues to update the JSON blob, and the database handles the rest.
- **Performance:** Because the data is "stored," you can add indexes to the generated column for faster access.

### Limitations

- **Deterministic:** The formula must produce the same result every time (e.g., you cannot use random number generators or current timestamps).
- **Scope:** The formula can only reference the current row.

### Conclusion

Generated columns are a powerful tool for "papering over" initial data modeling mistakes. They treat the extraction logic as data management rather than application business logic, ensuring data consistency and enabling better database performance.
