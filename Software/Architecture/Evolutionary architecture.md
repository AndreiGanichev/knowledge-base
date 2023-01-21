---
date: 2022-12-08
tags:
    - tag
---

An evolutionary architecture consists of three primary aspects: incremental change, fitness functions, and appropriate coupling.

### Incremental change

Требование incremental касаются изменений:
1. при разработке(*development*). Архитектура должна позволять вносить небольшие изменения
1. при поставке(*deployment*). Архитектура должна позволять выполнять небольшие поставки. Это возможность во многом обеспечивают практики *Continious Delivery*

### Fitness functions

Описывают целевые характеристики архитектуры. Держат архитектуру "в форме": не позволяют отклониться за определенные пределы. System-wide ff позволяют сравнить  две архитектуры как статически(какие результаты показывают ff), так и с точки зрения как архитектуры реагируют на определенные изменения.

### Appropriate coupling
*Coupling* неизбежен, потому что нельзя создать систему в изоляции. Наша цель - повышать *internal coupling* и снижать *external coupling*. Термин *internal coupling* употребляется в [[Evolutionary architectures book]] и, насколько я его понял, аналогичен [[Cohesion]].

> As in physics, four fundamental interactions exist in nature: gravitational, electromagnetic, strong, and weak. The strong nuclear force, which holds atoms (and therefore ordinary matter) together, is notable for its strength. Breaking it unleashes much of the power of nuclear fission. Similarly, some architectural components are extremely difficult to break into smaller pieces. Metaphorically, they exhibit strong nuclear force. One of the keys to building evolutionary architectures lies in determining **natural component granularity** and coupling bewteen components to fit the capabilities they want to support via the software architecture.

> For example, transactions act like a strong nuclear force, binding together otherwise unrelated pieces. While it is possible for developers to break apart a transactional context, it is a complex process and often leads to incidental complications like distributed transactions. Similarly, parts of a business might be highly coupled, and breaking the application into smaller architectural components may not be desirable.

Понятие appropriate coupling применимо на разных уровнях абстракции. На уровне сервисов оно связано с [[Architecture quantum]]. А внутри сервиса определяет внутреннюю архитектуру. [[Layered architecture]] основана на формировании слоев по техническому признаку. [[Modular monolith]] предполагает объединение в модуля по функциональному признаку.

---

### Источники:
1. [[Evolutionary architectures book]]

### Ссылки:
1. [[Continious Delivery]]
1. [[Architecture quantum]]#