# Characteristics of index

## Summary

### Introduction to Database Indexes

The video explains the fundamental mechanics of a database index using PostgreSQL as an example. By creating an index on a specific column (e.g., `room_number`), query performance changes from a slow sequential scan to a faster index scan.

### Core Characteristics of an Index

An index is defined by three primary traits:

- **Separate Data Structure:** It exists independently from the main table.
- **Partial Data Copy:** It contains a copy of only specific data from the main table.
- **Pointers:** It maintains pointers that link back to the corresponding rows in the main table.

### How Indexes Work

While data in a main table is often stored out of order (based on insertion time), an index (specifically a B-tree structure) stores the copied data in a sorted order.

- **Read Efficiency:** When a query requests a specific value (like room 1606), the database references the sorted index to quickly locate the value without scanning the entire table.
- **Retrieval:** Once the value is found in the index, the pointer directs the database to the exact row in the main table to retrieve the rest of the information.

### The Cost of Indexes

Indexes provide significant speed improvements for read operations but come with a maintenance cost.

- **Write Penalty:** Every time a row is inserted, updated, or deleted in the main table, the database must also update and re-sort the index structure.
- **Rule of Thumb:** You should create as many indexes as necessary to make the application fast, but as few as possible to minimize maintenance overhead. Since most database traffic is usually read-heavy (often 80-90%), trading write speed for faster read performance is generally an acceptable compromise.
