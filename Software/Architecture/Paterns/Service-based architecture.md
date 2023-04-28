---
date: 2023-01-24
tags:
    - tag
---
# Service-based architecture

В SBA также как и в [[Microservices]] сервисы domain centric, т.е. строятся с учетом предметной области.

Отличия от микросервисов:

1. обычно сервисы получаются бОльшими по размеру
1. часто БД остается монолитной
1. могут использовать integration middleware( может использоваться [[Enterprise Service Bus]]) для взаимодействия между сервисами

Эти отличия могут являться следствием того, что SBA можно рассматривать как промежуточный этап распиливания монолита. Монолитная БД позволяет сохранить транзакции, между поддоменами.

> Systems with complex transactional requirements map more cleanly to service-based architectures because of less stringent database requirements.

 ESB или медиатор позволяют упростить координацию работы сервисов. При этом не факт что после распиливания монолита на SBA нужно двигаться дальше в сторону микросервисов.

> Service-based architectures are a good compromise between the philosophical purity of microservices and the pragmatic realities of many projects. By loosening the strictures on service size, database independence, and incidental but useful coupling, this architecture solves the most painful aspects of microservices while preserving many of the benefits.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. link
