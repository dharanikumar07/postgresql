# Where to add index

## Summary

- **Indexing as an Art vs. Science:** While designing table schemas is a largely scientific process based on data shape and constraints, determining where to place indexes is more of an art form that requires judgment and iteration.
- **The Role of Access Patterns:** You cannot determine optimal indexing strategies by looking at the data alone. Instead, decisions must be driven by queries (access patterns). Understanding how data will be read—or knowing if it is read at all—is crucial.

### Key Query Elements to Analyze

When evaluating queries for potential indexes, focus on four main components:

- **Where Conditions:** Look for frequent filtering criteria. Strict equalities are prime candidates for B-tree indexes, followed by bounded or unbounded ranges.
- **Ordering (`ORDER BY`):** Indexes can significantly speed up sorting operations.
- **Grouping (`GROUP BY`):** Aggregations benefit from indexing.
- **Joins:** Columns used to join tables often require indexes for performance.

### Strategic Balance

The goal is to have as many indexes as necessary but as few as possible. It is generally inadvisable to blindly index every column found in a `WHERE` clause. A single well-thought-out index (e.g., on a strict equality) might be sufficient to handle multiple query variations without the overhead of maintaining additional indexes on range columns.

### Iterative Process

Indexing is not a "set it and forget it" task. As an application evolves, data distribution changes, and access patterns shift, developers must constantly revisit and refine their indexing strategy to ensure it remains efficient.
