---
date: 2022-06-16
tags:
    - tag
---
# Distributed algorithms basics

## Корректность алгоритма

Для разных алгоритмов корректность будет означать разное, но полезно формализовать подход для определения корретности. Для этого распределенные алгоритмы описываются свойствами. Но также важно понимать в какой системе работают алгоритмы. Чтобы не рассматривать каждую конкретную систему отдельно  т.е. ее [[system model]].

## Свойства распределенных алгоритмов

1. liveness - событие, предусмотренное алгоритмом должно произойти
1. safety - событие, не предусмотренное алгоритмом, не должно произойти

> If a **safety** property is violated, we can point at a particular point in time at which it was broken (for example, if the uniqueness property was violated, we can iden‐ tify the particular operation in which a duplicate fencing token was returned). After a safety property has been violated, the violation cannot be undone—the damage is already done.

> A **liveness** property works the other way round: it may not hold at some point in time (for example, a node may have sent a request but not yet received a response), but there is always hope that it may be satisfied in the future (namely by receiving a response).

### Другая трактовка

Интересно, что в [статье](https://decentralizedthoughts.github.io/2019-06-01-2019-5-31-models/) про [[Latency model]] термины используются в противоположном значении:

> recurring theme in the Partial synchrony model is to design protocols that are always safe (even when the system is asynchronous) but provide liveness and termination guarantees ... only when the system is synchronous.

---

## Источники

1. [[Database Internals book]]. Failure detection.
1. [[Designing Data-Intensive Applications book]]. Chapter 8. Knowledge, Truth and Lies.

## Ссылки

1. [[Leader election]]#
