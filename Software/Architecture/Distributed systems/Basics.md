---
date: 2022-06-15
---

# Distributed systems basics

Достоинства:

1. возможность [[Horizontal scaling]]
1. более высокие [[Reliability]] и [[Availability]] за счет *redundancy*
1. возможность снизить [[Latency]] за счет географического распределения
1. возможность обслуживания и остановки отдельных нод без остановки всей системы

Проблемы распределенных систем:

1. ненадежная сеть -> не можем гарантировать задержки меньше определенного значения (*unbounded delays*) (см. [[Latency model]])
1. паузы процессов
1. отсутствие [[Clock|глобальных часов]]

При переходе от одной ножы к распределенной системе существенно возрастает сложность:

1. требуется реализовать [[Load balancing]] между нодами
1. требуется реализовать [[Failure detection]], чтобы определять отказы в рамках выбранной [[Failure models]]
1. требуется учесть в алгоритмах, что возможны отказы отдельных нод (*partial failure*)
1. т.к. имеем дело с несколькими копиями данных требуется предоставить определенные гарантии в рамках выбранной [[Software/Architecture/Distributed systems/Consistency models/Basics|Consistency model]]
1. решения в распределенной системе не может принять одна нода, нужны распределенные алгоритмы: [[Consensus]], [[Quorum]]

> ...nondeterminism and possibility of partial failures is what makes distributed systems hard to work with.

---

## Источники

1. [[Designing Data-Intensive Applications book]]. Chapter 8.

## Ссылки

1. [Notes on Distributed Systems for Young Bloods](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/)
