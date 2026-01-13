---
date: 2022-12-18
tags:
    - tag
---
# Coupling

> Coupling speaks to how changing one thing requires a change in another. [2]

Полностью избавиться от coupling невозможно. Авторы книги Building evolutionary architectures используют понятие **appropriate coupling**. *Appropriate* или нет решается исходя из тех изменений, которые должна поддерживать архитектура конкретного проекта. Это понятие перекликается с [[Common Closure principle]].

> Decoupling comes with its own costs, both the cost of the decoupling itself and the future costs of unanticipated changes. **The more perfectly a design is adapted to one set of changes, the more likely it is to be blind-sided by novel changes**. Kent Beck [Monolith -> Services: Theory & Practice](https://medium.com/@kentbeck_7670/monolith-services-theory-practice-617e4546a879)

> **From a domain perspective**, a layered architecture has zero evolvability. In highly coupled architectures, change is difficult because coupling between the parts developers want to change is high. [[Building evolutionary architectures book]]. Chapter 4. Architectural Coupling

При этом слоеная архитектура облегчает эволюцию технических аспектов. В этом случае изменения касаются только конкретного слоя. Другое дело, что естественными являются изменений именно бизнес требований. Это особенно актуально для Core [[Subdomain]].

> No one perspective on architecture is “correct,” but rather a reflection on the goals developers build into their projects. If the focus is entirely on technical architecture, then making changes across that dimension is easier. However, if the domain perspective is ignored, then evolving across that dimension is no better than the [[Big ball of mud]].

> One of the major factors that impacts the ability to evolve an application at the architectural level is how *unintentionally coupled* each part of the system is.

## [[Appropriate coupling]]

*Coupling* неизбежен, потому что нельзя создать систему в изоляции. Наша цель - повышать *internal coupling* и снижать *external coupling*. Термин *internal coupling* употребляется в [[Building evolutionary architectures book]] и, насколько я его понял, аналогичен [[Cohesion]]. Там же *appropriate coupled module* фактически приравнивается к *functionally cohesive module* ([[Building evolutionary architectures book]]. Chapter 4. Modular monolith. Appropriate coupling).

## Как может проявляться coupling

Истоник: [1]. Chapter 5. Two-phase commit transaction

1. classes
1. package/namespace
1. library and framework
1. [[Database schema|data schemas]]
1. transactional contexts

Здесь можно вспомнить об идее [[Architecture dimensions]], которая акцентирует внимание на разных аспектах архитектуры, а не только на технической архитектуре(паттерны, фреймворки, библиотеки). Данные - это тоже важнейшее измерение в архитектуре и coupling проявляется и в этом измерении тоже. Причем для анализа технической архитектуры существет большое кол-во инструементов и IDE могут помочь обнаружить проблемы, то coupling на уровне данных гораздо сложнее обнаружить и распутать(*untangle*).

## Виды coupling в микросервисной архитектуре

Источник: [2]. Chapter 1. Just enough microservices

1. implementation: один микросервис зависит от реализации другого, например [[Shared database antipattern]]
2. temporal: для выполнения своей логики первый микросервис синхронно обращается ко второму микросервису. Это значит, что для выполнения логики первого нужно чтобы второй в этот момент времени был способен обслужить запрос.
3. deployment: деплой микросервис должен происходить вместе, один из признаков [[Distributed monolith]]. *Release train* - специальный процесс для проведения совместных релизов в таких случаях.
4. domain: когда нарушается принцип [[Information hiding]] и один микросервис получает доступ к данным, которые ему не нужны. Например, складскому сервису не нужны очень ограниченные данные о пользователе и составе заказа. Если сервис получает все данные заказа, то создается domain coupling.

### Transactions

> Transactions are a special form of coupling because transactional behavior doesn’t appear in traditional technical architecture-centric tools.

> While systems often cannot avoid transactions, architects should try to limit transactional contexts

> For example, transactions act like a strong nuclear force, binding together otherwise unrelated pieces. While it is possible for developers to break apart a transactional context, it is a complex process and often leads to incidental complications like distributed transactions. Similarly, parts of a business might be highly coupled, and breaking the application into smaller architectural components may not be desirable.

Если сделать микросервис слишком большим, то это потенциально создает излишне большой контекст транзакций -> неоправдано повышает coupling. Если слишком маленький(не соответсвует natural granularity) - то будет возникать необходимость в распределенных транзакциях между микросервисы, а значит они будут очень жестко связаны.

---

## Источники

1. [[Building evolutionary architectures book]]
1. [[Monolith to microservices book]]

## Ссылки

1. [[Architecture quantum]]
1. [[Architecture dimensions vs DDD Subdomains]]
1. [[Transaction]]
