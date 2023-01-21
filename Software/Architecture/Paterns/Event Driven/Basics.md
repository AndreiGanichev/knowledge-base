---
date: 2022-12-16
tags:
    - tag
---

> While asynchronous architectures create challenges around coordination and transactional behavior they offer fantastic parallel scale. [[Evolutionary architectures book]]

## Разновидности

1. **Broker EDA** предполагает наличие сообщений и их обработчиков. Обработчики независимые, их координация отсутствует.
1. **Mediator EDA** предполагает отдельную роль - медиатор, координирующий обработку сообщений. Все взаимодействие обработчиков ведется строго через медиатор, потому что он обладает знаниями о порядке операций, возможности их распараллеливания в ходе выполнения общего *workflow*. Наличие координатора облегчает обработку ошибок и понимание момента завершения workflow. Но при этом повышается *coupling* и размер архитектурного кванта.

---

### Источники:
1. link

### Ссылки:
1. [[Async vs sync]]
1. [DDD and Messaging Architectures. Mathias Verraes](https://verraes.net/2019/05/ddd-msg-arch/)
1. [[Event driven readability]]
1. [[Coupling]]
1. Designing Event-Driven Systems. Ben Stopford
1. Building Event-Driven Microservices. Adam Bellemare