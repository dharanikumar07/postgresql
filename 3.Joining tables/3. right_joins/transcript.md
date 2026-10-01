# Right joins

## Summary

- **Right Joins:** The speaker explains that a right join is functionally the mirror image of a left join; it returns all records from the "right" table and the matching overlap from the left. However, he argues that right joins are generally unnecessary. Because a right join is identical to a left join with the table order swapped, he recommends sticking to left joins, as they are more intuitive and standard for most developers.
- **Full Outer Joins:** This join type returns everything: all records from the left table, all records from the right table, and the matching overlap. Using the example of `users` and `posts`, this would result in a list containing orphaned posts, users who have never posted, and valid user-post matches. The speaker critiques this approach, suggesting it feels like mashing three distinct datasets together rather than providing a focused, useful result.
- **Cross Joins:** The transcript briefly mentions cross joins, which combine every row from one table with every row from another, but dismisses them as too esoteric for this overview.
- **Key Takeaway:** While it is good to know these alternative join types exist, the speaker emphasizes that inner joins and left joins are the "workhorses" of SQL and will handle the vast majority of use cases.
