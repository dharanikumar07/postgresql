# Updating JSON

## Summary

This video explains efficient methods for updating JSON data stored in PostgreSQL JSONB columns directly within the database, avoiding the inefficiency and race conditions associated with fetching, modifying, and re-saving entire JSON blobs application-side.

### The Problem with Application-Side Updates

The "naive" approach involves querying the database for the full JSON blob, modifying it in the application code, and sending the entire new blob back to the database. This is discouraged because:

- It requires unnecessary network round trips.
- It introduces race conditions where data might change between the read and the write, leading to stale data overwrites.

### Techniques for In-Place Updates

**The Merge Operator (`||`):**

- Used to update top-level keys or add new ones.
- Functions like a "JSON merge."
- Using the `RETURNING` clause allows you to see the result immediately in a single round trip.

```sql
UPDATE products
SET metadata = metadata || '{"price": 8.99}'
RETURNING *;
```

This updates only the `price` key while preserving the rest of the blob.

**The Delete Operator (`-`):**

- Used to remove a specific key from the JSON object.

```sql
metadata - 'price'
```

This removes the `price` key entirely.

**Updating Nested Objects (`jsonb_set`):**

- The merge operator (`||`) has a flaw with nested objects: it replaces the entire nested object rather than merging inside it (potentially deleting sibling keys like `cpu` when updating `ram`).
- To solve this, use the `jsonb_set` function:

```sql
jsonb_set(target_column, path_array, new_value)
```

- **Behavior:** It targets a specific path (e.g., `'{specs, ram}'`) and updates only that value without affecting the surrounding data. If the key does not exist, PostgreSQL will create it by default.

**Relative Updates:**

- You can update values based on their current state without knowing the value beforehand (e.g., increasing a price or decreasing stock).
- **Method:**
  1. Extract the current value using extraction operators.
  2. Cast the value to the correct data type (e.g., integer).
  3. Perform the calculation (e.g., `+ 100`).
  4. Wrap the result in `to_jsonb`.
  5. Insert it back using `jsonb_set`.
- This allows for atomic logic (like decrementing inventory) entirely within the database query.
