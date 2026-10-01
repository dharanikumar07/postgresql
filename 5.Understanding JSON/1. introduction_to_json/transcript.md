# Introduction to JSON

## Summary

PostgreSQL offers distinct advantages over competitors regarding JSON handling, specifically through two data types: `JSON` and `JSONB`. While both allow for storing JSON data, they serve different use cases based on how the database processes and stores the information.

### Key Takeaways

- **Best Practices for Storage:** Avoid storing all data in JSON columns. If data is well-structured and accessed frequently, it should be stored in standard table columns with appropriate constraints. JSON columns are best reserved for unstructured data or "blobs" where individual fields are rarely accessed.
- **The JSON Data Type:**
  - **Behavior:** Preserves the input exactly as received. This includes whitespace, ordering, and even duplicate keys (which standard JSON does not allow).
  - **Storage:** Stored as text.
  - **Use Case:** Ideal for logs or request payloads where maintaining an exact copy of the original input is necessary.
  - **Indexing:** Difficult to index effectively.
- **The JSONB Data Type:**
  - **Behavior:** Parses and cleans the data upon insertion. It trims whitespace, reorders keys (usually alphabetically), and removes duplicate keys (keeping the last value).
  - **Storage:** Stored in a decomposed binary format (hence the "B").
  - **Use Case:** Best for data that needs to be queried, processed, or manipulated within the database.
  - **Indexing:** Supports advanced indexing (like GIN indexes), making it significantly faster for querying specific fields within the JSON structure.
