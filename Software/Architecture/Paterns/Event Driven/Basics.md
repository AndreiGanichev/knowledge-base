---
date: 2022-12-16
tags:
    - tag
---
# Event driven basics

> While asynchronous architectures create challenges around coordination and transactional behavior they offer fantastic parallel scale. [[Building evolutionary architectures book]]

## Разновидности

[[Broker vs Mediator]]

## Достоинства [[Designing Data-Intensive Applications book]]. Chapter 4

1. очередь можно использовать как буфер на случай медленного или отказавшего получателя - повышает [[Reliability]] системы.
1. в случае неудачной обработки сообщение возвращается в очередь и не теряется
1. издатель отвязывается от получателя: как логически, так и физически отправителю не нужно знать IP получателя
1. одно сообщение может быть направлено нескольким получателям

### Не ведет к leaky abstraction

В отличие от [[RPC]] такой способ интеграции не является [[Leaky abstraction]] и обеспечивает *location transparency*: с точки зрения использования не отличается в случае интеграции между модулями внутри монолита и между микросервисами. Этот факт используется в [[Actor model(Orleans)]].

> Location transparency works better in the actor model than in RPC, because the actor model already assumes that messages may be lost, even within a single process. Although latency over the network is likely higher than within the same process, there is less of a fundamental mismatch between local and remote communication when using the actor model. [[Designing Data-Intensive Applications book]]. Chapter 4.

---

## Источники

1. link

## Ссылки

1. [[Latency model]]
1. [DDD and Messaging Architectures. Mathias Verraes](https://verraes.net/2019/05/ddd-msg-arch/)
1. [[Event driven readability]]#
1. Designing Event-Driven Systems. Ben Stopford
1. Building Event-Driven Microservices. Adam Bellemare
