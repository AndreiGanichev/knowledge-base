---
date: 2023-08-18
---
# Read your writes

> Read your writes, also known as read my writes, requires that if a process performs a write w, then that **same process** performs a subsequent read r, then r must observe w’s effects. [Jepsen](https://jepsen.io/consistency/models/read-your-writes)

## Реализация

1. читать данные, которые могут быть изменены пользователем, только с лидера.
1. отслеживать последнее время обновления сущности. По *replication log* реплик можно сразу понять какие из них содержат последнее обновление, а какие - отстают
1. запоминать время последнего обновления на клиенте. Это сложнее из-за трудности координировать время на разных нодах и тем более на клиенте. Кроме того пользователь может использовать разные устройства, что требует уже *cross-device* read-after-write-consistency.

---

## Источники

1. [[Designing Data-Intensive Applications book ]]. Chapter 5.

## Ссылки

1. [[Software/Architecture/Distributed systems/Consistency models/Basics]]
1. [[Load balancing]]
