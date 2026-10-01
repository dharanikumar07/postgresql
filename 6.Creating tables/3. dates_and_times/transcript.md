# Dates and times

## Summary

This summary provides an overview of handling date and time data types in PostgreSQL, with a specific focus on navigating time zone complexities.

### Primary Recommendation: timestamp with time zone

The most important takeaway is to use `timestamp with time zone` (or its shorthand `timestamptz`) for storing specific points in time.

- **How it works:** When data is inserted, PostgreSQL converts the value from its original time zone into UTC for storage. When the data is retrieved, it is converted from UTC to the time zone specified by the client connection.
- **Best Practice:** Keep data in UTC for as long as possible, ideally converting to the local time zone only at the client/front-end level.

### The Danger of Plain timestamp

The speaker advises against using the standard `timestamp` (or `timestamp without time zone`) unless the data is completely unrelated to real-world time zones (e.g., abstract wall clock time).

- **The Issue:** This data type stores the value exactly as entered without any conversion.
- **The Risk:** If you insert "6:00 PM" from a specific time zone (like America/Chicago) and retrieve it later via a connection set to UTC, it will still read "6:00 PM," resulting in a significant time calculation error.

### Other Temporal Data Types

- **`date`:** Suitable for storing a calendar date without a time component (e.g., birthdays, start/end dates).
- **`time`:** Stores a time of day without a date. The speaker notes this is less commonly useful in isolation.
- **Avoid Separation:** Do not store a single point in time as two separate columns (one for date, one for time), as this complicates math and time zone calculations.

### Additional Data Types Mentioned

- **Boolean:** A straightforward type for storing true or false values.

The speaker encourages reading the PostgreSQL documentation to build a strong foundation on these base data types.
