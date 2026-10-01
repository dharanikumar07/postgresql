# Data types

## Summary

This video provides an overview of foundational data types in PostgreSQL, emphasizing three guiding principles for schema design: keep columns small, simple, and honest (accurately representing the data and business rules).

### String Types

- **Text vs. Varchar:** Unlike MySQL conventions where `VARCHAR(255)` is common, PostgreSQL optimizes the `TEXT` type efficiently. It only occupies the space needed, making it a safe default for data of unknown length (e.g., names).
- **Enforcing Rules:** Use `VARCHAR(n)` only when you need to strictly enforce a business requirement, such as a state abbreviation (2 characters) or a SKU limit (20 characters). This allows the database to reject invalid data at the lowest level.

### Integer Types

- **Range Options:**
  - `SMALLINT`: Range of -32,768 to +32,767. Ideal for age or ratings (0-5).
  - `INTEGER`: Range of ~ ±2 billion. Good for counts like logins.
  - `BIGINT`: Range of ~ ±9 quintillion. Virtually unlimited; recommended for primary keys to prevent overflow.
- **Signed vs. Unsigned:** PostgreSQL does not support "unsigned" integers (positive only). All integer types allow negative numbers. To enforce positive-only values, developers must use a check constraint.

### Decimal Types

- **Numeric vs. Real:**
  - `NUMERIC`: Stores exact representations. This is required for financial data to avoid floating-point errors (e.g., ensuring `0.1 + 0.2` exactly equals `0.3`).
  - `REAL`: Stores approximate floating-point numbers. Suitable for scientific calculations but never for money.
- **Precision:** Numeric types can be defined with precision, such as `NUMERIC(10, 2)`. This specifies a total of 10 digits allowed, with 2 of those digits reserved for after the decimal point.
