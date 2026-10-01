# Aggregating data

## Summary

### Counting Rows and Values

- **`COUNT(*)`:** This is the most efficient way to count the total number of rows in a table. It is optimized by PostgreSQL to count rows as fast as possible without inspecting individual column data.
- **`COUNT(column_name)`:** This counts only the non-null values within a specific column. For example, `COUNT(id)` (a primary key) will match `COUNT(*)`, but `COUNT(phone)` will result in a lower number if some rows have null phone numbers.

### Aggregate Functions

- Beyond counting, SQL offers several aggregate functions including `SUM`, `AVG`, `MIN`, and `MAX`.
- These functions automatically ignore `NULL` values when calculating results, meaning you do not need to coalesce nulls manually before aggregation.

### Grouping Data (GROUP BY)

- **Basic Grouping:** Instead of running aggregates over an entire table, `GROUP BY` allows you to calculate statistics for specific subsets of data (e.g., total orders per `customer_id`).
- **Readability:** When using `GROUP BY`, it is best practice to include the column being grouped in the `SELECT` statement so the output represents a readable report rather than just a list of numbers.
- **Multiple Groups:** You can group by multiple criteria simultaneously. For instance, you can group by `customer_id` and the year (using `EXTRACT(YEAR FROM created_at)`) to see how many orders a specific customer placed in a specific year.
- **Filtering:** You can still use `WHERE` clauses to filter data. The filtering occurs before the grouping takes place.

### Common Grouping Errors

- **The Error:** A common error is "column must appear in the GROUP BY clause or be used in an aggregate function."
- **The Cause:** This occurs when you select a column that PostgreSQL is trying to "squash" into a single row but hasn't been told how to handle.
- **The Fix:** You must either add the column to the `GROUP BY` list or wrap the column in an aggregate function (like `SUM` or `COUNT`) so PostgreSQL knows how to process the data.
