# Btree structure

## Summary

### B-Tree Structure and Function

- **The Default Index:** The B-tree is the backing structure for approximately 99% of indexes in Postgres. While often visualized as a sorted list, it is actually a balanced tree structure.
- **Navigation:** Rather than scanning a list linearly, a B-tree allows the database to start at a root node and "hop" through intermediate nodes to quickly locate specific data at the bottom, significantly reducing lookup time.
- **Unique Constraints:** Postgres uses B-tree indexes behind the scenes to enforce primary keys and unique constraints. When a new record is inserted, the database checks the B-tree to ensure the value doesn't already exist, avoiding the need to scan the entire table.

### Performance and Usage

- **Sequential vs. Index Scans:** Without an index, looking up a value (e.g., by email) forces a "sequential scan," meaning the database reads every row in the table. Adding an index allows for an efficient "index scan."
- **Primary Use Cases:** B-trees are highly effective for:
  - **Strict Equality:** Looking up exact matches, e.g. `WHERE email = 'user@example.com'`.
  - **Ranges:** Both bounded and unbounded queries, e.g. `WHERE id BETWEEN 1 AND 10` or `created_at > '2025-01-01'`.
  - **Sorting:** Speeding up `ORDER BY` clauses.
  - **Joins:** Efficiently matching rows between tables (effectively a strict equality operation).

### Handling Text Patterns

- **The Wildcard Limitation:** By default, a standard B-tree index in Postgres will not speed up text pattern searches using wildcards (e.g., `LIKE 'name%'`).
- **The Solution (`text_pattern_ops`):** To support trailing wildcards, you must explicitly add an operator class when creating the index.
  ```sql
  CREATE INDEX index_name ON table (column text_pattern_ops);
  ```
  Note: if using `VARCHAR`, use `varchar_pattern_ops`. This modified index supports both wildcard searches and strict equality lookups.
- **Leading Wildcards:** B-trees cannot support leading wildcards (e.g., `LIKE '%name'`). To handle this, alternative strategies like indexing generated columns (e.g., extracting a domain from an email) are required to make the data indexable.
