# Operators and pattern matching

## Summary

This video tutorial expands on SQL filtering techniques, specifically focusing on handling lists of values and pattern matching within text.

### The IN Operator

Instead of writing long, inefficient chains of `OR` statements (e.g., `id=1 OR id=2 OR id=3`), use the `IN` operator.

```sql
WHERE id IN (1, 2, 3, 4)
```

- **Benefits:**
  - **Ergonomics:** It is easier to pass lists of IDs from application code directly into the query.
  - **Subqueries:** You can nest a `SELECT` query inside the parentheses (e.g., `WHERE id IN (SELECT id FROM ...)`). This allows PostgreSQL to optimize performance and avoids the "two round trips" anti-pattern where an application fetches IDs in one query and then requests data for those IDs in a second query.

### Pattern Matching

To filter text based on partial matches, use the `LIKE` operator with the `%` wildcard.

- **Wildcard Usage:**
  - `'A%'`: Starts with "A".
  - `'%A'`: Ends with "A".
  - `'%A%'`: Contains "A".
- **Case Sensitivity:**
  - `LIKE`: Is case-sensitive. `'A%'` will not match "aaron".
  - `ILIKE`: Is case-insensitive. `'a%'` will match both "Aaron" and "apple".
- **Strict Equality:** Standard equality (`=`) is case-sensitive. To handle case-insensitive strict equality without `ILIKE`, you can use the `LOWER()` function (e.g., `LOWER(name) = 'aaron'`), though this requires specific indexing strategies for performance.

### Complex Query Construction

The tutorial concludes by demonstrating how to combine these operators to build robust, readable queries without needing excessive parentheses for `OR` logic. A complex filter might combine:

- `IN` for checking against a list of countries.
- `ILIKE` for pattern matching names.
- `=` for strict status checking.
- `>=` and `<` for defining inclusive start and exclusive end date ranges.
