---
date: 2023-02-28
tags:
    - tag
---

## ООП

Р. Мартин в своей статье [Classes vs. Data Structures](https://blog.cleancoder.com/uncle-bob/2019/06/16/ObjectsAndDataStructures.html) противопотавляет классы и структуры данных:

1. Classes make functions visible while keeping data implied. Data structures make data visible while keeping functions implied.
1. Classes make it easy to add types but hard to add functions. Data structures make it easy to add functions but hard to add types.
1. Data Structures expose callers to recompilation and redeployment. Classes isolate callers from recompilation and redeployment.

Определене ООП от Алана Кея:

> The big idea is “messaging” […] The key in making great and growable systems is much more to design how its modules communicate rather than what their internal properties and behaviors should be.

## DDD

Тактические паттерны DDD([[Entity]], [[Aggregate]]) предполагают инкапсуляцию инвариантов и возможность изменять состояние только вызывая его публичные методы, т.е. используя поведение. Можно использовать [[CQRS]] и в предельном случае все данные в *Write Model* сделать приватными. Такой подход использует Kamil Grzbek.

## Архитектура

В [[Designing Data-Intensive Applications book]] говорится о том, что при проектировании архитектуры программной системы вы помимо прочего проектируете data system, потому что решаете в какой форме будут существовать данные(модели для чтения и для записи, кэш, хранилище для полнотекстового поиска и отчетов) и как они будут передвигаться между соответствующими хранилищами и трансформироваться.

> You are now not only an application developer, but also a data system designer.

---

### Источники:
1. link

### Ссылки:
1. [Closures And Objects Are Equivalent](http://wiki.c2.com/?ClosuresAndObjectsAreEquivalent)
1. [SOLID: the next step is Functional](https://blog.ploeh.dk/2014/03/10/solid-the-next-step-is-functional/)