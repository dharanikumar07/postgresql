# Explain

## Summary

### Purpose of EXPLAIN

Unlike imperative programming where you dictate exact steps, SQL is declarative; you request data, and the database determines the best way to retrieve it. The `EXPLAIN` keyword allows developers to peek "under the hood" to see the execution plan Postgres has generated for a specific query.

### Reading the Query Plan

- **Structure:** The output can be difficult to read because it represents a tree structure. It is often best read from the inside out or visualized using `FORMAT JSON`.
- **Nodes:** The plan consists of nodes (e.g., `Limit`, `Seq Scan`). An indented node with an arrow indicates it is a sub-process of the node above it.
- **Cost:** The plan provides a "Cost" metric. These are arbitrary units (not time) representing the startup and total effort required to run the query.

### Impact of Indexes

The video demonstrates the difference between a sequential scan and an index scan using a query searching for a specific email:

- **Sequential Scan:** Without an index, Postgres scans the entire table (100,000 rows). The cost was approximately 2,400 units.
- **Index Scan:** After creating an index on the email column, the plan shifted to an index scan. The cost dropped to 2 units, demonstrating a massive improvement in efficiency.

### EXPLAIN ANALYZE

- **Function:** Unlike standard `EXPLAIN`, `EXPLAIN ANALYZE` executes the query. It provides actual execution times (in milliseconds) and accurate row counts rather than just estimates.
- **Critical Warning:** Because it runs the query, be extremely cautious when using this with data modification commands like `UPDATE` or `DELETE`.
- **Performance Insight:** In the example, the query without an index took 695ms and inefficiently filtered out 99,999 rows after checking them one by one. With the index, execution time dropped to 9ms.

### Sequential Scan Caveats

While often slower, a sequential scan is not always the wrong choice. It may be preferable when:

- The table is very small (it is faster to just read the whole thing than load index structures).
- The query returns a large percentage of the table (e.g., 10-20%), where the overhead of utilizing an index outweighs the benefits.
