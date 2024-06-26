---
date: 2024-03-26
---
# Inversion of control

Часто в разговорах о IoC упоминают принцип Голливуда: "Мы сами вам перезвоним"

## Unix tools

Мартин Клепман, описывая философию unix и принципы, которые позволяют формировать pipe из несокльких утилит, отмечает, что это достигается в том числе благодаря следованию принципу IoС. Утилиты используют ```stdin```/```stdout``` для ввода и вывода вместо того, чтобы самим определять конкретный источник или устройство вывода. Источник и вывод определяются уже при написании конкретной команды, что также является примером [позднего связыывания](https://en.wikipedia.org/wiki/Late_binding).

---

## Источники

1. [[Designing Data-Intensive Applications book]]. Chapter 10. Separation of logic and wiring.

## Ссылки

1. link
