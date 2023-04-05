---
date: 2023-03-15
tags:
    - tag
---

Разные структуры данных по разному реализуются на диске и в памяти(*main memory*). Эффективность реализации определяет целесообразность использования той или иной структуры.

> *Disk-based* databases use specialized storage structures, optimized for disk access. *In memory*, pointers can be followed comparatively quickly, and random memory access is significantly faster than **the random disk access**. *Disk-based* storage structures often have a form of wide and short trees, while *memory-based* implementations can choose from a larger pool of data structures and perform optimizations that would otherwise be impossible or difficult to implement on disk. Similarly, handling **variable-size data** on disk requires special attention, while in memory it’s often a matter of referencing the value with a pointer. [[Database Internals book]]. Chapter 1.

В продолжении мысли о том, что in-memory структуры имеют бОльший выбор:

> Maintaining a sorted structure on disk is possible (see “B-Trees”), but maintaining it in memory is much easier. There are plenty of well-known tree data structures that you can use, such as *red-black trees or AVL trees*. [[Designing Data-Intensive Applications book]].Chapter 3.

---

### Источники:
1. link

### Ссылки:
1. [[B-tree]]