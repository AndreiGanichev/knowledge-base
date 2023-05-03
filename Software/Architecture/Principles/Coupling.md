---
date: 2022-12-18
tags:
    - tag
---
# Coupling

Полностью избавиться от coupling невозможно. Авторы книги Building evolutionary architectures используют понятие **appropriate coupling**. *Appropriate* или нет решается исходя из тех изменений, которые должна поддерживать архитектура конкретного проекта. Это понятие перекликается с [[Common Closure principle]].

> Decoupling comes with its own costs, both the cost of the decoupling itself and the future costs of unanticipated changes. **The more perfectly a design is adapted to one set of changes, the more likely it is to be blind-sided by novel changes**. Kent Beck [Monolith -> Services: Theory & Practice](https://medium.com/@kentbeck_7670/monolith-services-theory-practice-617e4546a879)

> **From a domain perspective**, a layered architecture has zero evolvability. In highly coupled architectures, change is difficult because coupling between the parts developers want to change is high. [[Building evolutionary architectures book]]. Chapter 4. Architectural Coupling

При этом слоеная архитектура облегчает эволюцию технических аспектов. В этом случае изменения касаются только конкретного слоя. Другое дело, что естественными являются изменений именно бизнес требований. Это особенно актуально для Core [[Subdomain]].

> No one perspective on architecture is “correct,” but rather a reflection on the goals developers build into their projects. If the focus is entirely on technical architecture, then making changes across that dimension is easier. However, if the domain perspective is ignored, then evolving across that dimension is no better than the [[Big ball of mud]].
> One of the major factors that impacts the ability to evolve an application at the architectural level is how *unintentionally coupled* each part of the system is.

## Appropriate coupling from [[Building evolutionary architectures book]]

*Coupling* неизбежен, потому что нельзя создать систему в изоляции. Наша цель - повышать *internal coupling* и снижать *external coupling*. Термин *internal coupling* употребляется в [[Building evolutionary architectures book]] и, насколько я его понял, аналогичен [[Cohesion]]. Там же *appropriate coupled module* фактически приравнивается к *functionally cohesive module* ([[Building evolutionary architectures book]]. Chapter 4. Modular monolith. Appropriate coupling).

> As in physics, four fundamental interactions exist in nature: gravitational, electromagnetic, strong, and weak. The strong nuclear force, which holds atoms (and therefore ordinary matter) together, is notable for its strength. Breaking it unleashes much of the power of nuclear fission. Similarly, some architectural components are extremely difficult to break into smaller pieces. Metaphorically, they exhibit strong nuclear force. One of the keys to building evolutionary architectures lies in determining **natural component granularity** and coupling bewteen components to fit the capabilities they want to support via the software architecture.

Понятие appropriate coupling применимо на разных уровнях абстракции. На уровне сервисов оно связано с [[Architecture quantum]]. А внутри сервиса определяет внутреннюю архитектуру. [[Layered architecture]] основана на формировании слоев по техническому признаку. [[Modular monolith]] предполагает объединение в модуля по функциональному признаку.

В [[Layered architecture]] разбиение сделано по техническому признаку(architecture dimensions). Это *intentional* coupling. Но знания о предметной области оказываются распределены между слоями, что создает *unintentional* coupling. То есть естественные силы/естественная гранулярность связывает их(поэтому их приходится менять вместе), несмотря на то, что их разделили по слоям.

> one of the important aspects of an evolvable architecture is appropriate coupling across dimensions.

Это значит, что после того как определили важные в конкретном проекте *architecture dimensions*, необходимо проектировать архитектуру так, чтобы изменения в этих измерениях были легкими.

> First make the change easy, then make the easy change. Kent Beck

При этом ничего не мешает использовать слоеную архитектуру внутри микросервиса, просто она будет инкапсулирована внутри физических границ сервиса и не будет никак связана с технической архитектурой других сервисов.

## Как может проявляться coupling

Истоник: [[Building evolutionary architectures book]]. Chapter 5. Two-phase commit transaction)

1. classes
1. package/namespace
1. library and framework
1. [[Database schema|data schemas]]
1. transactional contexts

Здесь можно вспомнить об идее [[Architecture dimensions]], которая акцентирует внимание на разных аспектах архитектуры, а не только на технической архитектуре(паттерны, фреймворки, библиотеки). Данные - это тоже важнейшее измерение в архитектуре и coupling проявляется и в этом измерении тоже. Причем для анализа технической архитектуры существет большое кол-во инструементов и IDE могут помочь обнаружить проблемы, то coupling на уровне данных гораздо сложнее обнаружить и распутать(*untangle*).

### Transactions

> Transactions are a special form of coupling because transactional behavior doesn’t appear in traditional technical architecture-centric tools.

> While systems often cannot avoid transactions, architects should try to limit transactional contexts

> For example, transactions act like a strong nuclear force, binding together otherwise unrelated pieces. While it is possible for developers to break apart a transactional context, it is a complex process and often leads to incidental complications like distributed transactions. Similarly, parts of a business might be highly coupled, and breaking the application into smaller architectural components may not be desirable.

Если сделать микросервис слишком большим, то это потенциально создает излишне большой контекст транзакций -> неоправдано повышает coupling. Если слишком маленький(не соответсвует natural granularity) - то будет возникать необходимость в распределенных транзакциях между микросервисы, а значит они будут очень жестко связаны.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. [[Architecture quantum]]
1. [[Architecture dimensions vs DDD Subdomains]]
1. [[Transaction]]
