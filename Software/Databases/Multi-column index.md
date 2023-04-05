---
date: 2023-03-13
tags:
    - index
---

> This is like an old-fashioned paper phone book, which provides an index from (lastname, first‐ name) to phone number. [[Designing Data-Intensive Applications book]]. Chapter 3.


> A multicolumn B-tree index can be used with query conditions that involve any subset of the index's columns, but the index is most efficient when there are constraints on the leading (leftmost) columns. **The exact rule is that equality constraints on leading columns, plus any inequality constraints on the first column that does not have an equality constraint, will be used to limit the portion of the index that is scanned**. Constraints on columns to the right of these columns are checked in the index, so they save visits to the table proper, but they do not reduce the portion of the index that has to be scanned. For example, given an index on (a, b, c) and a query condition WHERE a = 5 AND b >= 42 AND c < 77, the index would have to be scanned from the first entry with a = 5 and b = 42 up through the last entry with a = 5. Index entries with c >= 77 would be skipped, but they'd still have to be scanned through. This index could in principle be used for queries that have constraints on b and/or c with no constraint on a — but the entire index would have to be scanned, so in most cases the planner would prefer a sequential table scan over using the index. [PostgreSQL documentation](https://www.postgresql.org/docs/current/indexes-multicolumn.html)

---

### Источники:
1. link

### Ссылки:
1. #[[Index]]