# Accessing JSONB data

## Summary

The following summary outlines the methods and best practices for accessing data within JSON fields in PostgreSQL.

### JSON vs. JSONB Access

- **JSONB:** Fields can be accessed and traversed directly because the data is stored in a binary format that PostgreSQL understands.
- **JSON:** Fields cannot be accessed directly. The column must first be cast to JSONB because standard JSON is stored simply as text. Storing data as JSONB is highly recommended for this reason.

### Basic Access Operators

- **Single Arrow (`->`):** Extracts a specific key from the JSON blob but returns the result as a JSONB object (e.g., strings will remain quoted). This is used for traversing through nested structures.
- **Double Arrow (`->>`):** Extracts a key and unquotes it, returning the result as text. This is typically used at the end of a chain to get the final value.

### Path Syntax (Alternative Notation)

- **Hash Single (`#> '{path,to,key}'`):** Equivalent to chaining single arrows. It follows a specified path array and returns a JSONB object.
- **Hash Double (`#>> '{path,to,key}'`):** Equivalent to ending a chain with a double arrow. It follows the path and returns the final value as text.
- **Benefit:** This syntax is often cleaner when accessing deeply nested keys (e.g., 10 layers deep).

### Type Casting and Safety

- **Casting:** Since the double arrow (`->>`) returns text, numeric values extracted from JSON must be explicitly cast to the correct type (e.g., `::integer`) before performing mathematical comparisons or operations.
- **Safety:** Both access methods are safe. If a query attempts to access a key or path that does not exist, PostgreSQL will simply return `NULL` rather than causing an error.

### Querying Best Practices

- **Where Clauses:** Extracted JSON fields can be used in `WHERE` clauses. However, proper type casting is essential to avoid errors (e.g., comparing a text result `"99"` against an integer `100`).
- **Schema Design:** While useful for metadata, frequently accessed or critical fields (like `price`) should ideally be moved to top-level columns rather than remaining inside the JSON blob to avoid the overhead of extraction and casting.
