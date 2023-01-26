---
date: 2022-12-16
tags:
    - distributed monolith
    - code reuse
---

Это SOA важной частью которой является *Enterprise Service Bus*(ESB).

### Элементы архитектуры

1. Business service(BS) - отражают абстрактные функции системы
1. Enterprise service(ES) - содержат конкретную реализацию, к которой обращается ESB в рамках выполнения workflow. Проектируются для переиспользования
1. ESB
1. Application service - не настолько важный функционал и имеет небольшую степень переиспользования, чтобы делать его уровня enterprise
1. Infrastructure service (logging, metrics)

### Функции ESB

1. шина для обмена сообщениями между серивсам и **координация** работы сервисов в рамках выполнения *workflow*(см. Mediator EDA [[Software/Architecture/Paterns/Event Driven/Basics]])
    a. хореография BS
    b. оркестрация ES
1. трансформация сообщений, которыми обмениваются сервисы в случае, если они используют разные протоколы

### Характеристики

Краеугольные принципы ESB архитектуры - **переиспользование** и разделение ресурсов. В идеале, формируется набор ES для выполнения различных конкретных шагов и потом новый функционал реализуется путем комбинации этих готовых шагов.

Переиспользование идет вразрез со способностью эволюционировать. [[Architecture quantum]] данной архитектуры огромный, потому что для реализации и поставки бизнес функционала придется затронуть большинство элементов архитектуры. Сложность добавляет и распределенная природа архитектуры. То есть получаем распределенный монолит.

Тестирование такой архитектуры также затруднено - сервисы не получается протестировать в изоляции, потому что они не реализуют какой-то логически законченный функционал, а являются частью глобального workflow. Поэтому в основном применимы e2e тесты.

### Почему такая архитектура вообще имела место

Само существование ESB пораждает вопросы - зачем вообще было создавать такой паттерн, который по своей сути настолько не поддерживает изменения. Одно из объяснений - возможности и паттерны использования инфраструктуры развертывания того времени. Это пример того, что на архитектура не живет в изоляции, а определяется множеством факторов, в том числе особенностями среды развертывания.

> For example, when SOA was a popular architectural style, companies didn’t use tools like open-source operating systems — all infrastructure was commercial, licensed, and expensive. A decade ago, a developer proposing a microservices architecture, where every service runs on its own instance of an operating system and machine, would be laughed out of the operations center because the architecture would have been ludicrously expensive. Because of the dynamic equilibrium of the software development ecosystem, new architectures arise because of a literal new environment.

---

### Источники:
1. [[Building evolutionary architectures book]]. Architectural Coupling

### Ссылки:
1. [[Architecture quantum]]
1. [[History]]