# Dealing with NULLs

## Summary

### Core Concept: Null is Unknown

The most fundamental rule of working with `NULL` in PostgreSQL is understanding that null is not a value. It is not equivalent to false, an empty string, or zero. Instead, `NULL` represents an unknown state or a "black box."

### Three-Valued Logic

Unlike traditional programming which relies on True/False logic, SQL introduces a third state: Unknown.

- **Standard Logic:** `5 = 5` is `True`; `5 = 10` is `False`.
- **Null Logic:** `5 = NULL` results in `NULL` (Unknown).
- **Null Equality:** Crucially, `NULL = NULL` also results in `NULL`. You cannot know if one unknown value equals another unknown value.

### Filtering Data

Because SQL queries only return rows where the `WHERE` clause evaluates to strictly `True`:

- **Incorrect:** `WHERE phone = NULL` will return zero results. The expression evaluates to `NULL` (Unknown), not `True`, so the database filters those rows out.
- **Correct:** You must use the specific operators `IS NULL` or `IS NOT NULL` to filter for these values.

### Handling Nulls with COALESCE

The `COALESCE` function is used to handle `NULL` values gracefully in query results.

- **Functionality:** It accepts multiple arguments and returns the first non-null value found in the list.
- **Usage:** It is useful for providing fallbacks:
  ```sql
  COALESCE(phone, 'No phone given')
  ```
  or strictly logic-based substitutions, like displaying a username if a display name is missing:
  ```sql
  COALESCE(name, username)
  ```

### Nulls in Arithmetic

Mathematical operations involving `NULL` will always result in `NULL` (e.g., `10 + NULL = NULL`). To prevent gaps in data or broken calculations, wrap nullable columns in a coalesce function to provide a default mathematical value, such as `COALESCE(column, 0)`.
