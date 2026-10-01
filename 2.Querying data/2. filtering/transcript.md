# Filtering

## Summary

This video tutorial expands on SQL filtering techniques within the `WHERE` clause, covering equality, inequalities, ranges, and best practices for using conjunctions.

### Key Filtering Methods

- **Strict Equality:** Uses the `=` operator.
  ```sql
  WHERE name = 'Aaron'
  ```
- **Inequalities:** There are two valid syntaxes: the SQL standard `<>` and the programmer-centric `!=`. Both function identically to filter out specific values.
- **Ranges:** Used to filter data between values using greater than (`>`), less than (`<`), and their inclusive counterparts (`>=`, `<=`).
  - These are commonly used with numeric columns (e.g., filtering IDs).
  - They are also applicable to other data types, such as dates (e.g., `created_at`) and strings (e.g., filtering names alphabetically).

### Compound Filters and Parentheses

Conjunctions: multiple conditions can be combined using `AND` and `OR`.

- **Order of Operations:** The `AND` operator has higher precedence than `OR`. This creates "implicit parentheses" that can lead to logic errors if not carefully managed.
- **Best Practice:** The speaker strongly advocates for the liberal use of explicit parentheses when combining `AND` and `OR`.
  - **Readability:** Makes the query's intent immediately obvious to teammates.
  - **Accuracy:** Prevents logical errors caused by misremembering operator precedence (e.g., ensuring an `OR` condition applies to the correct grouping).
  - **Performance:** PostgreSQL compiles the query down effectively regardless of extra parentheses, so there is no performance penalty for adding them for clarity.
