# Left joins

## Summary

### Concept and Purpose

- Unlike an inner join which finds the overlap between two tables, a left join retrieves all records from the first (left) table and brings along associated data from the second (right) table if it exists.
- If there is no matching data in the right table, the query returns the user data with `NULL` values for the columns from the right table.

### Syntax and Structure

- The structure is similar to other joins but explicitly uses the `LEFT JOIN` keyword.
- The table listed first (in the `FROM` clause) is the "left" table, and the query works outwards to the right.

```sql
SELECT *
FROM users
LEFT JOIN posts ON users.id = posts.user_id;
```

In this case, `users` is the left table. The `ON` clause is required to specify how the tables relate (e.g., matching primary and foreign keys).

### Key Use Cases Demonstrated

- **Retrieving All Primary Records:** You can list every user, regardless of whether they have written a post or not. If a user has written multiple posts, the user data is duplicated for each post row.
- **Finding "Orphaned" or Missing Data:**
  - **Users without posts:** By filtering for `WHERE posts.id IS NULL`, you can isolate users who have not contributed any content.
  - **Posts without users:** By flipping the order (making `posts` the left table) and filtering for `WHERE users.id IS NULL`, you can find "orphaned" posts that exist without an associated user (e.g., after a user account was deleted).

### Best Practices

- While the order of columns in the `ON` clause (e.g., `users.id = posts.user_id` vs. `posts.user_id = users.id`) does not strictly matter, it is a common preference to list the left table's column first.
- Filtering out `NULL` values in a left join essentially replicates the behavior of an inner join, showing that SQL often offers multiple ways to achieve the same result.
