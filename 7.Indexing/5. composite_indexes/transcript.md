# Composite indexes

## Summary

### Composite vs. Single Column Indexes

A common failure pattern in database design is creating multiple single-column indexes rather than one composite index. While Postgres is capable of using two separate indexes (e.g., one for `status` and one for `country`) and finding the overlap, it is inefficient.

- **Recommendation:** Create a single composite index across multiple columns (e.g., `country` and `status`).
- **Mechanism:** This creates a single B-tree sorted first by the primary column, then by the secondary column, allowing the database to drill down directly to the relevant data without comparing separate result sets.

### The "Leftmost Prefix" Rule

The order in which columns are defined when creating the index is critical (unlike the order in which parameters are written in a query, which does not matter). Index utility works strictly from left to right.

- **How it works:** If you have an index on `(Country, Status, Email)`:
  - Querying just `Country` uses the index (leftmost prefix formed).
  - Querying `Country` + `Status` uses the index.
  - Querying `Status` + `Email` (skipping `Country`) does not efficiently use the index because the leftmost prefix is missing.
- **No Skipping:** You cannot skip a column in the middle of the index. If you query `Country` and `Email` but skip `Status`, the database can use the index for `Country`, but it cannot efficiently use the index to filter `Email`.

### Handling Range Conditions

Index traversal stops optimizing at the first range condition (e.g., `>`, `<`, `BETWEEN`).

- **Rule:** Strict equalities (e.g., `country = 'US'`) should be placed at the beginning (left side) of the index definition.
- **Placement:** Range conditions (e.g., `date > '2025-01-01'`) should be placed at the end. Once the database encounters a range query on a column, it cannot use subsequent columns in the index for further access operations.

### Postgres 18 Update: Skip Scans

Postgres 18 introduced "Skip Scans," which mitigates the penalty of missing a leftmost prefix.

- If you skip the first column of an index in your query, Postgres 18 can still utilize the index more effectively than a full table scan.
- **Caveat:** Despite this improvement, relying on Skip Scans is still slower than a properly designed index that adheres to the leftmost prefix rule. Design best practices remain unchanged.
