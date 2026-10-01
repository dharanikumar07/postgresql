# Intro to indexes

## Summary

This video introduces the concept of indexing in PostgreSQL, explaining its purpose and functionality using real-world analogies.

### The "Hotel PostgreSQL" Analogy

- **Without an Index:** Finding a specific room (e.g., 412) is like taking an elevator to the first floor, checking every door, then moving to the second floor, and repeating the process until the room is found. This is inefficient.
- **With an Index:** An index acts like hotel signage. It allows you to go directly to the correct floor (Floor 4) and immediately know which direction to turn (left or right) to find the specific room, bypassing unrelated doors entirely.

### Database Behavior

- **Sequential Scan:** When no index is present, the database performs a "sequential scan," walking through every single row in a table to find a match. This is comparable to checking every hotel door or reading every page of a book to find a specific word. It is very slow on large tables.
- **Index Scan:** An index allows the database to skip irrelevant data and jump directly to the location of the requested information, significantly improving speed.

### Alternative Analogies

The transcript briefly mentions phone books (jumping to the 'F' section for "Francis") and book indexes (looking up "vacuum" in the back of a book to find the page number) as other ways to visualize how database indexes function.
