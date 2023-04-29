---
date: 2022-12-12
tags:
    - tag
---
# Fitness functions

> Architects design architectures, but then expose them to the messy real world of *implementing* things atop the architecture.

ФФ обеспечивают требование, что изменения должны быть не только *incremental*, но и *guided*.

> Without guidance, evolutionary architecture becomes simply a reactionary architecture.

Существуют разные способы проверки на соответствие нефунекциональным требованиям(метрики, тесты, ручные проверки, практики). Концепция ФФ позволяет унифицировать разные виды проверок и воспринимать их как **единый мехнизм** соответствия архитектуры нефункциональным требованиям.

## Типы

1. *Key* - ключевые измерения(*dimensions of architecture*) и соответствующие фф - напрямую влияют на технологические и архитектурные решения
1. *Relevant* - важные фф, которые принимаются во внимание при реализации фич, но не влияют напрямую на важные решения. Например, метрики, оценивающие качество кода, code style.
1. *Not relevant* - эти измерения не влияют на принятие архитектурных и технологических решений

Необходимо концентрироваться в первую очередь на ключевых измерениях и сделать изменения в этих направлениях простыми. Это обеспечит возможность экспериментировать и оптимизировать архитектуру для достижения требуемых характеристик, контролируемых фитнес функциями.

## Кто владеет

> Fitness functions can have any owner, including shared ownership. ...the application team may own the directionality(*прим. направление зависимостей между слоями*) fitness function because it is a particular concern for that project. In the same deployment pipeline, fitness functions common across multiple projects may be owned by the security team. In general, the definition and maintenance of fitness functions is a **shared responsibility between architects, developers, and any other role concerned with maintaining architectural integrity**.

Мартин Фаулер описывает роль [[Architect]] как гида(*guide*), который *mentor the development team*. Одним из инструментов выполнения такой функции менторинга могут быть фф.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. #[[Evolutionary architecture]]
