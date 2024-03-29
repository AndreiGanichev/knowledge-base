---
date: 2023-08-18
---
# Consistency: CAP vs ACID

В продолжении того, что термин consistency сильно перегружен можно сравнить, что именно он означает в этих абревиатурах.

ACID является совокупностью требований к транзакциям СУБД. Под консистентностью понимается гарантия того, что транзакция всегда переводит систему из одного валидного состояния в другое. Валидность состояния состоит в **соблюдении всех инвариантов БД**: *constraints*, *referential data integrity*.

В CAP консистентность эквивалентна [[Linearizability]](конкретной [[Software/Architecture/Distributed systems/Consistency models/Basics|consistency model]])

---

## Источники

1. [[Database Internals book]]. Replication and Consistency.

## Ссылки

1. [[CAP theorem]]
