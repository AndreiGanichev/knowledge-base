---
date: 2023-08-18
---
# Write follow read

> Writes follow reads, also known as session causality, ensures that if a process reads a value v, which came from a write w1, and later performs write w2, then w2 must be visible after w1. *Once you’ve read something, you can’t change that read’s past*. [1]

> This is a particular problem in partitioned (sharded) databases. [2]
---

## Источники

1. [Jepsen](https://jepsen.io/consistency/models/writes-follow-reads)
1. [[Designing Data-Intensive Applications book ]]. Chapter 5.

## Ссылки

1. #[[Software/Architecture/Distributed systems/Consistency models/Basics]]
