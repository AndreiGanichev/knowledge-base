---
date: 2022-12-17
---
# Duplication vs Coupling

## Duplication

> Microservices form a [[Shared nothing architecture]] — the goal is to decrease coupling as much as possible. Generally, **duplication is preferable to coupling**.

[[Building evolutionary architectures book]]. Architectural Coupling

Это утверждение в первую очередь касаются *данных* и логики уровня [[Application layer]].

## Доменная логика

Примером выбора в пользу *duplication* является [[Separate ways]].
Примером выбора в пользу *coupling* является [[Cooperation]]. Shared Kernel.

## Инфраструктура

Дублирование *инфраструктуры* противоречит [[Remove needless variability]]:

> Duplicating technical architecture functionality across services creates a slew of well-known problems.

[[Building evolutionary architectures book]]. Chapter 6. Case study: Service template.

В этом случае выбирают *coupling*, а не *duplication* и используют [[Service template]].

---

## Источники

1. link

## Ссылки

1. [[Coupling]]
1. [[Cohesion]]
