# More generated columns

## Summary

### Generated Columns in PostgreSQL

Generated columns act similarly to calculated columns in spreadsheet software like Excel or Google Sheets. They allow you to define a column's value based on a formula involving other columns within the same row. PostgreSQL manages the maintenance of this value, ensuring consistency and preventing direct user modification.

### Key Concepts and Benefits

- **Data Consistency:** By moving logic (such as concatenating a first and last name into a "full name") from the application layer to the database layer, you ensure the calculation is always consistent across all queries and applications.
- **Determinism:** Formulas for generated columns must be deterministic, meaning they cannot use functions that return changing values (like `RAND()` or `NOW()`). They must also reference only the current row, prohibiting cross-row calculations.

### Types of Generated Columns

Starting with PostgreSQL 18, users can choose between two storage strategies:

- **`VIRTUAL`:** The value is calculated at runtime every time it is queried. It is not written to disk and cannot be indexed. This is ideal for simple, low-cost display calculations (e.g., combining names).
- **`STORED`:** The value is calculated upon insertion or update and physically written to the disk. While this consumes storage space, it allows the column to be indexed.

### Use Cases

- **Simplifying Data Access:** Create columns that format data for display (e.g., `fullname`) without complex query syntax in every application call.
- **Indexing Complex Data:** Generated columns are particularly useful for indexing specific parts of a string that are otherwise hard to query efficiently.
  - Example: Extracting an email domain (e.g., `@example.com`) into a stored generated column allows for indexing and efficient querying of user segments (like grouping by company domain) without complex regex operations during the query.
