# Having

## Summary

Here are the key takeaways from the video regarding filtering grouped data in SQL:

- **Filtering Groups vs. Rows:** The `WHERE` clause cannot be used to filter aggregate results (like a count) because `WHERE` is applied before grouping occurs to eliminate individual rows.
- **The HAVING Clause:** To filter data after it has been grouped, the `HAVING` clause must be used. It acts as a filter specifically for groups rather than rows.

### Syntax and Order of Operations

1. `WHERE` clauses appear before `GROUP BY` and eliminate specific rows.
2. `GROUP BY` organizes the remaining rows into groups.
3. `HAVING` appears after `GROUP BY` and eliminates entire groups based on aggregate conditions, e.g.:
   ```sql
   HAVING COUNT(*) >= 50
   ```

### PostgreSQL Specific Restriction

Unlike some other databases (like MySQL or SQLite), PostgreSQL requires you to repeat the aggregate function in the `HAVING` clause. You cannot refer to an aggregate by the alias created in the `SELECT` statement.

```sql
-- Correct
HAVING COUNT(*) > 50

-- Incorrect (cannot use alias)
HAVING order_count > 50
```
