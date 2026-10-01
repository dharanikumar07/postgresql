# Check constraints

## Summary

### The Core Principle

The speaker advocates for setting a solid foundation by making impossible states impossible at the lowest level—the database. While application-level validation is necessary, database constraints act as a final safeguard against bad data entering the system (e.g., via admin interfaces or direct SQL editing).

### Check Constraints

- Use the `CHECK` keyword to enforce logical rules on data columns.
- Example: to prevent negative prices, a column constraint like `CHECK (price >= 0)` ensures that any insertion of a negative value results in a database error.

### Table-Level Constraints

- Constraints can enforce logic between two related columns within the same row.
- Example: a "Sale Price" should logically be lower than the "Regular Price." This can be enforced with a check that verifies `(sale_price < price)` or allows `sale_price` to be null.

### Naming Constraints

- Postgres generates default names for constraints (e.g., `products_check`), which can be vague in error logs.
- It is recommended to explicitly name constraints using the syntax:
  ```sql
  CONSTRAINT [name] CHECK (...)
  ```
  This provides clear, descriptive error messages (e.g., `sale_price_must_be_lower`) that help developers identify the exact violation.

### Philosophy on Logic in the Database ("Bounds of Reasonableness")

- Do not put complex, changeable business logic (like holiday-specific date rules) in the database.
- Instead, enforce "bounds of reasonableness" to catch obvious errors.
- **Examples of Reasonable Bounds:**
  - An "End Date" must be after a "Start Date."
  - A rating must be between 1 and 10.
  - An age should be between 0 and 120.
  - An email address must contain an `@` symbol (a basic sanity check rather than full RFC compliance).
