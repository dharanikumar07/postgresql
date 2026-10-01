# Unions

## Summary

### Comparison of JOIN vs. UNION

- **JOINs:** Combine two tables side-by-side, creating wider rows with columns from both sources.
- **UNIONs:** Combine two result sets over-under (stacking them vertically), creating a result with more total rows.

### Use Case: Archive Tables

- It is common practice to separate data into "current" and "archive" tables (e.g., `current_employees` vs. `former_employees`) to keep the active dataset small and performant.
- A `UNION` allows you to query both tables simultaneously to generate a unified list (e.g., getting names and emails for all employees).

### Rules for Using UNION

- **Column Count:** Both queries being unioned must return the exact same number of columns.
- **Data Types:** The columns in the same position must share the same data type (e.g., you cannot stack an integer column on top of a string column).
- **Literals:** You can use string literals to satisfy column count requirements or to add metadata (e.g., adding a `type` column to label rows as `'current'` or `'former'`).

### Handling Duplicates (UNION vs. UNION ALL)

- **`UNION` (Default):** Automatically scans for and eliminates duplicate rows. This process has a performance cost.
- **`UNION ALL`:** Includes all rows, even duplicates. It is faster and cheaper for the database because it skips the duplicate checking step.
- **Best Practice:** Use `UNION ALL` if you know duplicates are impossible based on business logic, or if you explicitly want to retain duplicates. Use `UNION` only when deduplication is necessary.
