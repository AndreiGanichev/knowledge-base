---
date: 2023-09-11
---
# Clock

## Wall clock

> ...wall clock time, which represents the time of the day, is measured by a clock machinery generally built with an crystal oscillator. [Wall clock not monotonic](https://martinfowler.com/articles/patterns-of-distributed-systems/time-bound-lease.html#wall-clock-not-monotonic)

Проблема в том, что частота генератора может меняться, например она зависит от температуры окружающей среды. Чтобы синхронизировать время существует [[Network Time Protocol]].

> Physical clocks, also called wall clocks, are bound to a notion of time in the physical world and are accessible through process- local means; for example, through an operating system. [[Database Internals book]]. Part II. Basic definitions

## Logical clock

> Logical clocks are implemented using a kind of monotonically growing counter. [[Database Internals book]]. Part II. Basic definitions

1. [[Vector clock]]
1. [[Lampord clock]]

---

## Источники

1. link

## Ссылки

1. link
