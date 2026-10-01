# Exploring the data

## Summary

Here is a summary of the video transcript:

### Getting Started with SQL Queries

This session focuses on the foundational aspects of writing SQL queries, distinguishing between exploring data ("poking around") and writing production-ready code.

### Key Concepts and Best Practices

**`SELECT *` (Select Star):**

- **Usage:** Selecting all columns (`SELECT *`) is excellent for exploration and initial data analysis.
- **Production Context:** While often discouraged in raw SQL due to performance overhead (fetching unnecessary data), it is acceptable when using Object-Relational Mappers (ORMs) like ActiveRecord or Prisma.
- **ORMs:** ORMs typically use `SELECT *` to fully populate objects, preventing issues where code later attempts to access unselected, null fields. However, developers remain responsible for the SQL their ORM generates.

**Limiting Data:**

- When exploring large datasets, always use the `LIMIT` clause to prevent the database from attempting to return millions of rows, which can crash the connection or application.

**Column Selection and Renaming:**

- **Select Only What You Need:** In raw SQL production code, it is best practice to specify only the necessary columns (e.g., `SELECT name, email`) to reduce network load.
- **Aliasing:** You can rename columns for clarity using the `AS` keyword (e.g., `SELECT created_at AS registration_date`). This is particularly useful for calculated columns or concatenations that otherwise default to confusing names like `?column?`.

**Clarity is Kindness:**

- The transcript emphasizes writing explicit code to help future maintainers (and yourself).
- Although optional, using the `AS` keyword for aliases and specifying `ASC` (ascending) or `DESC` (descending) in order clauses makes code easier to read and parse mentally.

**Ordering Results:**

- **Deterministic Ordering:** Without an `ORDER BY` clause, the database may return results in any order. To ensure consistency, always specify an order.
- **Multiple Columns:** You can order by multiple columns to handle ties. For example, `ORDER BY name ASC, id DESC` ensures that if two names are identical, they are sorted secondarily by ID.

### The Fundamental SQL Pattern

The session concludes by outlining the core pattern that developers will encounter repeatedly:

```sql
SELECT (columns)
FROM (table)
WHERE (conditions)
ORDER BY (sorting)
LIMIT (restriction)
```
