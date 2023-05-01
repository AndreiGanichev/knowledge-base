---
date: 2023-05-01
tags:
    - tradeoff
---
# Broker vs Mediator

1. **Broker EDA** предполагает наличие сообщений и их обработчиков. Обработчики независимые, их координация отсутствует.
1. **Mediator EDA** помимо сообщений и обработчиков предполагает отдельную роль - *медиатор*, координирующий обработку сообщений. Все взаимодействие обработчиков ведется строго через медиатор, потому что он обладает знаниями о порядке операций, возможности их распараллеливания в ходе выполнения общего *workflow*. Наличие координатора облегчает реализацию поведения похожего на транзакции, обработку ошибок и понимание момента завершения workflow. Но при этом повышается [[Coupling]] и размер [[Architecture quantum]].

> This is a classic example of an *architectural tradeoff*. Broker EDAs offer many advantages in terms of evolvability, asynchronicity, scale, and a host of other desirable characteristics. However, common tasks like transactional coordination become more difficult.

---

## Источники

1. [[Building evolutionary architectures book]]. Chapter 4. Event-Driven Architectures.

## Ссылки

1. link
