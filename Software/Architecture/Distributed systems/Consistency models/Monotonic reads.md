---
date: 2023-08-18
---
# Monotonic reads

> Monotonic reads ensures that if a process performs read r1, then r2, then r2 cannot observe a state prior to the writes which were reflected in r1; intuitively, *reads cannot go backwards*. [Jepsen](https://jepsen.io/consistency/models/monotonic-reads)

## Реализация

1. Балансировать запросы конкретного пользователя на одну и ту же реплику.

---

## Источники

1. [[Designing Data-Intensive Applications book ]]. Chapter 5.

## Ссылки

1. #[[Software/Architecture/Distributed systems/Consistency models/Basics]]
1. [[Load balancing]]
