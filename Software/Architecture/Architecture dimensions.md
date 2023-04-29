---
date: 2023-01-27
tags:
    - tag
---
# Architecture dimensions



> dimensions of architecture — the parts of architecture that fit together in often orthogonal ways. Some dimensions fit into what are often called architectural concerns (the list of “-ilities” above), but dimensions are actually broader, encapsulating things traditionally outside the purview of technical architecture. Each project has dimensions the architect must consider when thinking about *evolution*(т.е. добавлем еще одно измерение - *время*).

Помимо *функциональных требований*, которые должна обеспечивать система, в обычно в архитектуре приложений выделяют другие важные измерения:

1. Technical: frameworks, dependent libs, implementation languages
1. [[Evolutionary data]]#
1. Security
1. Operational/System

Если целостность функциональных требований контролируется юнит, функциональными и интеграционными тестами, то целостность других измерений контролируется с помощью [[Fitness functions]].

> To build an evolvable system, architects must think about how the system might evolve across all the important dimensions.

Еще одним измерением, я думаю, можно рассматривать организационную структуру, которая, как известно благодаря [[Conway's law]] оказывает влияние на архитектуру, точнее на ее возможность эволюционировать.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. [[Architecture dimensions vs DDD Subdomains]]#
