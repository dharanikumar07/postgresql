# Upserting data

## Summary

### The Problem: Managing Unknown Record States

When handling user data (such as preferences), developers often face a scenario where they need to insert a record if it doesn't exist, or update it if it does. A common but inefficient approach involves a two-step process:

1. **Select:** Query the database to check if the record exists.
2. **Logic:** If yes, run an `UPDATE`; if no, run an `INSERT`.

Why this is "bad":

- **Performance:** It requires two separate network round trips between the application and the database.
- **Race Conditions:** It creates a timing gap between the read and the write operations, leading to potential data inconsistencies if multiple requests happen simultaneously.

### The Solution: The Upsert

The efficient alternative is to perform an Upsert (Update + Insert), which combines both logic paths into a single atomic query. This prevents race conditions and reduces database load.

In PostgreSQL, this is achieved using the `INSERT INTO ... ON CONFLICT` syntax.

### Implementation Strategies

The speaker outlines three distinct ways to handle data conflicts using `ON CONFLICT`:

- **Overwrite with New Data:** When a unique key conflict occurs (e.g., the `user_id` already exists), you can instruct the database to update specific columns using the `excluded` pseudo-table. The `excluded` table contains the values that would have been inserted had the conflict not occurred.
  - Example: If a user changes their theme from `'light'` to `'dark'`, the query attempts to insert `'dark'`. If the user exists, it grabs `'dark'` from the `excluded` table and updates the existing record.
- **Do Nothing:** If the goal is to ensure a record exists without changing current data, you can use `DO NOTHING`.
  - Example: Attempt to insert a default preference. If the user already has a preference set, the database simply ignores the new request and leaves the existing data untouched.
- **Calculate/Increment Values:** You are not required to use the `excluded` values during a conflict. You can reference the existing data in the table to perform calculations.
  - Example: For an analytics counter, you can attempt to insert a view count of 1. If the record exists, you can set the new value to `existing_views + 1`. This allows for incrementing counters in a single step without first reading the current count.
