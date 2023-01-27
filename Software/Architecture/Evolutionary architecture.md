---
date: 2022-12-08
tags:
    - tag
---

> To build an evolvable system, architects must think about how the system might evolve across all the important dimensions.


An evolutionary architecture consists of three primary aspects: incremental change, fitness functions, and appropriate coupling.

### Incremental change

Требование incremental касаются изменений:
1. при разработке(*development*). Архитектура должна позволять вносить небольшие изменения
1. при поставке(*deployment*). Архитектура должна позволять выполнять небольшие поставки. Это возможность во многом обеспечивают практики *Continious Delivery*

### Fitness functions

Описывают целевые характеристики архитектуры. Держат архитектуру "в форме": не позволяют отклониться за определенные пределы. System-wide ff позволяют сравнить  две архитектуры как статически(какие результаты показывают ff), так и с точки зрения как архитектуры реагируют на определенные изменения.

### Appropriate coupling

см. [[Coupling]]


> Evolution is easier if developers have created a modular component system with well- defined integration points.

---

### Источники:
1. [[Building evolutionary architectures book]]

### Ссылки:
1. [[Continious Delivery]]
1. [[Architecture dimensions]]
1. [[Architecture quantum]]#