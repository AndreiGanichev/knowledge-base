---
date: 2023-09-11
---
# Clock

## Wall (time-of-day) clock

> current date and time according to some calendar (also known as wall-clock time) [[Designing Data-Intensive Applications book]]. Chapter 8. Monotonic versus time-of-day clock.

> ...wall clock time, which represents the time of the day, is measured by a clock machinery generally built with an crystal oscillator. [Wall clock not monotonic](https://martinfowler.com/articles/patterns-of-distributed-systems/time-bound-lease.html#wall-clock-not-monotonic)

Проблема в том, что частота генератора может меняться, например она зависит от температуры окружающей среды. Чтобы синхронизировать время существует [[Network Time Protocol]]. Но синхронизация через NTP может заставлять часы перескакивать назад или вперед, в зависимости от того спешат часы на ноде или отстают. Поэтому такие часы нельзя использовать для определения длительности какого-то интервала на ноде. Для этого нужно использовать *monotonic clock*.

> Physical clocks, also called wall clocks, are bound to a notion of time in the physical world and are accessible through process- local means; for example, through an operating system. [[Database Internals book]]. Part II. Basic definitions

### Event ordering

Wall clock нельзя использовать для определения порядка событий в распределенной системе, потому что несмотря на наличие NTP существует вероятность того, что расхождение часов на разных нодах (например мастера multi-master системы) буде значительным. Что может привести к ошибкам. Например, в случае использования стратегии [[Write conflicts|LWW]], возможна потеря данных. Для этих целей используются logical clock, которые могут обеспечить [[Causal cosistency]].

## Monotonic clock

Подходят для измерения длительности, но их абсолютное значение не имеет никакого смысла. Также бессмысленно сравнивать значения монотонных часов на разных машинах.

Монотонные часы, как следует из названия, идут только вперед. Они могут синхронизироваться по NTP, но они не прыгают вперед или назад, вместо этого немного замедляются или ускоряются при необходимости.

## Logical clock

> Logical clocks are implemented using a kind of monotonically growing counter. [[Database Internals book]]. Part II. Basic definitions

1. [[Vector clock]]
1. [[Lampord clock]]

---

## Источники

1. link

## Ссылки

1. link
