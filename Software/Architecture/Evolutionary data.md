---
date: 2023-01-27
tags:
    - tag
---

> While it’s true that schemas change less frequently than code, database schemas still represent an abstraction of the real world.

> Evolutionary design in databases occurs when developers can build and evolve the structure of the database as requirements change over time.

> The key to evolving database design lies in evolving schemas alongside code.

> Rather than make a schema change and risk breaking existing systems, they instead just add a new table, joining it to the original using relational database primitives...When DBAs don’t restructure the database, they’re not preserving a precious enterprise resource, they’re instead creating the concretized remains of every version of the schema, all overlaid upon one another via join tables.

> While data abstractions resist change better than behavior, they must still evolve. Architects must treat data as a primary concern when building an evolutionary architecture.

> When you combine several tools in order to provide a service(*прим. когда разрабатываем приложение*), the service’s interface or application programming interface (API) usually hides those implementation details from clients. Now you have essentially created a new, special-purpose data system from smaller, general-purpose components. Your composite data system may provide certain *guarantees*: e.g., that the cache will be correctly invalidated or upda‐ ted on writes so that outside clients see consistent results. **You are now not only an application developer, but also a data system designer.** [[Designing Data-Intensive Applications book]]


Эволюция схемы должна производится так, чтобы обеспечить [[Compatibility]].
> ...you can think of storing something in the database as sending a message to your future self. [[Designing Data-Intensive Applications book]]. Chapter 4.

---

### Источники:
1. [[Building evolutionary architectures book]]]

### Ссылки:
1. #[[Architecture dimensions]]