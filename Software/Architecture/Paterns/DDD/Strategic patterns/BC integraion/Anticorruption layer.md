---
date: 2023-05-09
---
# Anticorruption layer

Суть паттерна состоит в том, чтобы проксировать все общение с UBC через ACL, который будет выступать переводчиком между UL DBC и контрактами, которые предоставляет UBC. В случае если несколько DBC реализуют у себя ACL для общения с одним и тем же UBC, то есть смысл вместо этого перенести место перевода на сторону UBC(если есть такая возможность) и использовать паттерн OHS.

## Когда использовать

1. downstream [[Bounded context]]] (DBC) - это core [[Subdomain]]
1. модель upatream BC (UBC) совсем не пригодна для исопльзования в DBC
1. контракты UBC часто меняются

## Реализация

ACL может быть реализован как часть компонента DBC, так и выделен в отдельный BC - **interchange context**

---

## Источники

1. [[Learning DDD book]]. Chapter 4. Integrating bounded contexts.
1. [[Learning DDD book]]. Chapter 9. Communication patterns. Model translation.

## Ссылки

1. link
