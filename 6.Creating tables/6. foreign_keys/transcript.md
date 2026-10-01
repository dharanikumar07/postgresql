# Foreign keys

## Summary

Here is a summary of the video transcript:

### Foreign Keys and Referential Integrity

- **Definition:** A foreign key is a primary key from one table that resides in another table to establish a link between the two (e.g., a `user_id` column in a `posts` table linking to the `users` table).
- **Constraint vs. Concept:** There is a distinction between the concept of a foreign key and a foreign key constraint. The concept is simply the relationship, while the constraint is the database rule enforcing that the referenced data exists.
- **Referential Integrity:** By applying a constraint (e.g., `references users(id)`), the database ensures data quality by preventing the insertion of "orphaned" data. For example, a post cannot be created with a `user_id` that does not exist in the `users` table.

### ON DELETE Behaviors

When a parent record (e.g., a user) is deleted, the foreign key constraint determines how the database handles the associated child records (e.g., posts). The available options include:

- **`RESTRICT` (Default):** The database prevents the deletion of the parent record if child records exist.
- **`CASCADE`:** Deleting the parent automatically deletes all associated child records. This flows down the chain (e.g., deleting a Team deletes Projects, which deletes Threads, which deletes Comments).
- **`SET NULL`:** The parent is deleted, and the foreign key column in the child records is set to `NULL`, creating orphaned records rather than deleting them.
- **`SET DEFAULT`:** The foreign key column reverts to its default value upon parent deletion.

### Risks and Scalability

- **The Cascade Danger:** While convenient, `ON DELETE CASCADE` can be dangerous. Deleting a single high-level entity (like a Team) could unintentionally wipe out millions of rows of related data across multiple tables.
- **Performance at Scale:** Foreign key constraints can impact performance at very high scales because the database must verify the existence of the parent record with every insertion. However, for most applications, `RESTRICT` is a safe and recommended starting point.
