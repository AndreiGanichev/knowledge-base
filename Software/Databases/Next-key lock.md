---
date: 2023-12-02
---
# Next-key lock

Другое название *index-range lock*.

С помощью такой блокировки можно избавиться от [[ACID Isolation|phantoms]] аномалии.

Next-key lock можно считать более грубой, но и более производительной альтернативой [[Predicate lock]]. Вместо того, чтобы каждый раз вычислять предикат **блокируется часть индекса**, которая заведомо обеспечит блокировку нужного диапазона записей, но при этом скорее всего заблокирует и лишние данные.

> In the room bookings database you would probably have an index on the `room_id` column, and/or indexes on `start_time` and `end_time`. Say your index is on `room_id`, and the database uses this index to find existing bookings for room 123. Now the database can simply attach a shared lock to this index entry, indicating that a transaction has searched for bookings of room 123.

---

## Источники

1. [[Designing Data-Intensive Applications book ]] Chapter 7. Transactions

## Ссылки

1. [[MySql locks|Использование в MySql]]
