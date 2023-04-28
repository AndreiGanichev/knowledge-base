---
date: 2022-12-12
tags:
  - tag
---
# Abstraction vs complexity

> There is no theoretical reason that anything is hard to change about software. If you pick any one aspect of software then you can make it easy to change, but we don’t know how to make everything easy to change. **Making something easy to change makes the overall system a little more complex**, and making everything easy to change makes the entire system very complex. Complexity is what makes software hard to change. That, and duplication. Ralf Johnson. [Who needs architect](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf)

Чаще всего для того, чтобы обеспечить возможность изменений в том или ином аспекте используют [[Dependency Inversion principle]] и вводят абстракцию, реализация которой как раз может меняться.

Но абстракция не является [[Silver bullet]]:

1. добавление абстракции может усложнить чтение, навигацию по коду, когда нужно понять что действительно происходит в том или ином сценарии - повышает _complexity_. Примеры:
   1. использование паттерна [[Mediatr]] вместо непосредственных вызовов компонентов друг друга
   2. [[Event driven readability]]
1. добавляя абстракцию мы решаем вопрос изменчивост только в одном направлении(в одной плоскости)

> Объектно-ориентированное решение на основе полиморфизма(_прим. рисование разных фигур_) позволяет легко расширять функциональность лишь в определенном направлении, но не является открытым к любым изменениям. Задача добавления новой операции в существующую иерархию типов решается с помощью паттерна [[Visitor]](_прим. реализация сложнее, чем на основе полиморфизма_)...Паттерн Visitor показывает функциональный подход к расширяемости семейства типов. [[Паттерны проектирования на платформе .NET]]. Часть 4.

> No one perspective on architecture is “correct,” but rather a reflection on the goals developers build into their projects. If the focus is entirely on technical architecture, then making changes across that dimension is easier. However, if the domain perspective is ignored, then evolving across that dimension is no better than the Big Ball of Mud. [[Building evolutionary architectures book]]

> First make the change easy, then make the easy change. Kent Beck

---

## Источники

1. link

## Ссылки

1. [[Abstraction]]
1. [[Complexity]]
1. [[Open close principle]]
1. [[Coupling]]
