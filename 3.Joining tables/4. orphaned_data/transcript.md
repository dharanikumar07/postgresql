# Orphaned data

## Summary

### Warning Against Premature Use of INNER JOINs

The speaker advises caution when defaulting to `INNER JOIN` for tasks such as displaying website posts and their authors. While an inner join correctly links data, it will exclude "orphaned" records—such as posts where the associated user account has been deleted (e.g., due to GDPR requests).

### Key Takeaways and Recommendations

- **Clarify Business Requirements:** Before writing the query, determine if the goal is to show only complete records (post + author) or all primary records (posts) regardless of whether secondary data exists.
- **The Risk of INNER JOIN:** Using an inner join can cause valid content to disappear unexpectedly from the front end if the related data changes or is deleted over time.
- **Use LEFT JOIN for Completeness:** To ensure all primary content remains visible, use a `LEFT JOIN`. This guarantees every post is retrieved, even if the user ID is null.
- **Handle Null Values with COALESCE:** When using a left join, null values may appear for missing authors. Use the `COALESCE` function to replace nulls with a user-friendly placeholder:
  ```sql
  COALESCE(users.name, 'Unknown Author')
  ```
  This prevents display errors on the front end while ensuring data integrity.
