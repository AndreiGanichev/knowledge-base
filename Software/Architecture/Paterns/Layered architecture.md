---
date: 2023-04-24
tags:
    - tag
---

# Layered architecture

> Most applications are built by layering one data model on top of another. For each layer, the key question is: how is it represented in terms of the next-lower layer?...each layer hides the complexity of the layers below it by providing a clean data model. [[Designing Data-Intensive Applications book]]. Chapter 2.

Т.о. каждый слой должен формировать [[Abstraction|абстракцию]].

## Слой доступа к данным

[[Data models]]

К этому слою относятся как БД, так и очереди и брокеры событий, но они предоставляют принципиально разные паттерны доступа.

---

## Источники

1. link

## Ссылки

1. link
