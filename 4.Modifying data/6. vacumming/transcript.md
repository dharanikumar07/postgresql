# Vacuuming

## Summary

Here is a summary of the key concepts regarding vacuuming in Postgres:

### Purpose and MVCC

- **Feature Specificity:** Vacuuming is a feature specific to Postgres (not found in MySQL) that handles data versioning.
- **Multi-Version Concurrency Control (MVCC):** Postgres uses MVCC to manage data. When a row is updated, the old version is not immediately deleted. Instead, it is marked as "dead," and a new version is written. This allows multiple readers and writers to interact with the database concurrently without conflict.
- **Accumulation of "Dead" Data:** As data is updated over time (Version 1 → Version 2 → Version 3), the database file grows because dead versions occupy space on the disk.

### The Role of Vacuuming

- **Reclaiming Space:** The vacuum process scans the database file, identifies "dead" rows (tuples), and marks their blocks as free space.
- **Space Reuse vs. Shrinking:** Standard vacuuming generally does not shrink the physical file size on the disk. Instead, it makes the file "sparse," creating empty blocks within the existing file that Postgres can reuse for future data writes.
- **Statistics Updates:** Beyond cleaning up space, vacuuming analyzes the "shape" of the data to update internal statistics. The query planner uses these statistics to make informed decisions, such as which indexes to use for optimal performance.

### Operational Takeaways

- **Manual Execution:** You can run a vacuum manually using the command:
  ```sql
  VACUUM table_name;
  ```
- **Autovacuum:** In most cases, manual execution is unnecessary. Postgres has an autovacuum feature that is enabled by default (and standard on hosted providers). It automatically handles cleanup and statistics updates based on internal heuristics.
- **Tuning:** While autovacuum works well for most use cases, advanced users may eventually need to tune autovacuum settings as their database scales.
