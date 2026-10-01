# Guiding principles

## Summary

This video focuses on the "Create" phase of the CRUD lifecycle, specifically the process of defining table schemas. While creating tables is often the last step a developer learns when onboarding to an existing project, it serves as the critical foundation for any new application.

### Defining "Schema"

The speaker clarifies that in this context, "schema" refers to the structure of a specific table (columns, data types, and constraints), rather than the high-level organizational folders within a Postgres database.

### The Developer's Responsibility

Regardless of whether a developer uses full-stack frameworks, AI tools, or hand-written SQL to generate migrations, they are ultimately responsible for the output. The speaker emphasizes that developers must understand and approve the underlying SQL being generated.

### Three Rules of Thumb for Table Creation

When defining columns, developers should adhere to three guiding principles:

- **Pick the Smallest Data Type:** Choose the smallest type that will accommodate the entirety of the expected data range (e.g., do not use a large integer type for an "Age" column when a smaller type suffices).
- **Pick the Simplest Data Type:** Use native types to reduce overhead. For example, do not store numbers as strings or wrap simple data in JSON, as this forces the database to perform unnecessary conversions during calculations.
- **Pick the Most Realistic Representation:** The schema should accurately reflect reality. If data is required, explicitly set the column to `NOT NULL`; if data might be missing, allow nulls.

### The Rationale: Performance over Storage

The drive to use small, efficient data types is not about saving disk space, which is inexpensive. Instead, it is about optimizing RAM usage and indexing. Tightly packed data on the disk fits more efficiently into the cache and results in smaller, faster indexes, significantly improving overall database performance.
