---
date: 2022-12-16
tags:
    - tag
---

> While asynchronous architectures create challenges around coordination and transactional behavior they offer fantastic parallel scale. [[Building evolutionary architectures book]]

## Разновидности

1. **Broker EDA** предполагает наличие сообщений и их обработчиков. Обработчики независимые, их координация отсутствует.
1. **Mediator EDA** помимо сообщений и обработчиков предполагает отдельную роль - *медиатор*, координирующий обработку сообщений. Все взаимодействие обработчиков ведется строго через медиатор, потому что он обладает знаниями о порядке операций, возможности их распараллеливания в ходе выполнения общего *workflow*. Наличие координатора облегчает реализацию поведения похожего на транзакции, обработку ошибок и понимание момента завершения workflow. Но при этом повышается [[Coupling]] и размер [[Architecture quantum]].

## Достоинства

В отличие от [[RPC]] такой способ интеграции не является [[Leaky abstraction]] и обеспечивает *location transparency*: с точки зрения использования не отличается в случае интеграции между модулями внутри монолита и между микросервисами. Этот факт используется в [[Actor model]].

> Location transparency works better in the actor model than in RPC, because the actor model already assumes that messages may be lost, even within a single process. Although latency over the network is likely higher than within the same process, there is less of a fundamental mismatch between local and remote communication when using the actor model. [[Designing Data-Intensive Applications book]]. Chapter 4.

---

### Источники:
1. link

### Ссылки:
1. [[Async vs sync]]
1. [DDD and Messaging Architectures. Mathias Verraes](https://verraes.net/2019/05/ddd-msg-arch/)
1. [[Event driven readability]]#
1. Designing Event-Driven Systems. Ben Stopford
1. Building Event-Driven Microservices. Adam Bellemare