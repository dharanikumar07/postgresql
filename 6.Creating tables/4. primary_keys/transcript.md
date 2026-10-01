# Primary keys

## Summary

Here is a comprehensive summary of the video transcript regarding primary keys:

### Core Definition and Purpose

- Every table should have a primary key to enforce uniqueness and non-nullability.
- This ensures every single row is uniquely identifiable.
- **Recommendation:** Use "synthetic" primary keys (numbers assigned to the row) rather than "natural" keys (real-world data like SSNs, license plates, or emails), as real-world data can change or be reused.

### Preferred Data Types

The speaker identifies two valid options for primary key data types:

**BIGINT (The "Best" Option):**

- This is the speaker's top recommendation.
- Unlike the general rule of "smallest data type possible," primary keys are an exception. Always use `BIGINT` instead of standard integers to avoid running out of keys (citing Basecamp's outage as a cautionary tale).
- **Preferred Syntax:**
  ```sql
  id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY
  ```
  This is the modern SQL standard, replacing the older `serial` pseudo-type.
- This method puts the database in charge of assigning IDs.

**UUID (The "Okay" Option):**

- Valid for specific use cases, particularly when IDs must be generated on the client-side or without central coordination (e.g., optimistic UI).
- **Crucial Caveat:** If using UUIDs, specifically use Variant 7. This version is time-ordered, avoiding performance issues associated with random UUIDs.

### Composite Keys

- The speaker advises against using composite primary keys (using multiple columns to form a unique identifier).
- **Alternative:** If business logic requires uniqueness across multiple columns (e.g., Author + Team), create a separate unique constraint on those columns instead. Stick to a single `BIGINT` column for the actual primary key to keep referencing simple.
