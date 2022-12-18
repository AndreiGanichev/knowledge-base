---
date: 2022-12-18
tags:
    - tag
---

Полностью избавиться от coupling невозможно. Авторы книги Building evolutionary architectures используют понятие **appropriate coupling**. *Appropriate* или нет решается исходя из тех изменений, которые должна поддерживать архитектура конкретного проекта. Это понятие перекликается с [[Common Closure principle]].

> Decoupling comes with its own costs, both the cost of the decoupling itself and the future costs of unanticipated changes. **The more perfectly a design is adapted to one set of changes, the more likely it is to be blind-sided by novel changes**. Kent Beck [Monolith -> Services: Theory & Practice](https://medium.com/@kentbeck_7670/monolith-services-theory-practice-617e4546a879)

> **From a domain perspective**, a layered architecture has zero evolvability. In highly coupled architectures, change is difficult because coupling between the parts developers want to change is high. Ford, Parson, Kua. Building evolutionary architectures. Chapter 4. Architectural Coupling

При этом слоеная архитектура облегчает эволюцию технических аспектов. В этом случае изменения касаются только конкретного слоя. Другое дело, что естественными являются изменений именно бизнес требований. Это особенно актуально для Core [[Subdomain]].

> No one perspective on architecture is “correct,” but rather a reflection on the goals developers build into their projects. If the focus is entirely on technical architecture, then making changes across that dimension is easier. However, if the domain perspective is ignored, then evolving across that dimension is no better than the Big Ball of Mud. 
---

### Источники:
1. link

### Ссылки:
1. [[Arch Quantum]]