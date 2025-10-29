---
date: 2022-12-17
tags:
    - tag
---
# Microservices

> microservices are independently deployable services modeled around a [[Subdomain | business domain]]. They communicate with each other via networks. We use the principles of [[Information hiding]] together with domain-driven design to create services with stable boundaries that are easier to work on independently, and we do what we can to reduce the many forms of [[Coupling]].

## История появления термина

> Back in 2011, when I was still working at a consultancy called ThoughtWorks, my friend and then-colleague James Lewis became really interested in something he was calling “micro-apps.” He had spotted this pattern being utilized by a few companies that were using service-oriented architecture—they were optimizing this architecture to make services easy to replace. The companies in question were interested in getting specific functionality deployed quickly, but with a view that it could be rewritten in other technology stacks if and when everything needed to scale. At the time, the thing that stood out was how small in scope these services were. Some of these services could be written (or rewritten) in a few days. James went on to say that “services should be no bigger than my head.” The idea being that the scope of functionality should be easy to understand, and therefore easy to change. Later, in 2012, James was sharing these ideas at an architectural summit where a few of us were present. In that session, we discussed the fact that really these things weren’t self-contained applications, so “micro-apps” wasn’t quite right. Instead, “microservices” seemed a more appropriate name. [2]

## Достоинства

> The ability for developers to deploy one service without affecting any other service is one of the defining benefits of this architectural style.[1]

Для того, чтобы новая версия микросервиса работала с текущими версиями других сервисов, необходимо при развитии микросервисов учитывать требования [[Compatibility|совместимости]].

## [[Coupling]]

> Architects often call microservices a **“share nothing”** architecture. The primary advantage of this architecture style is no coupling at the technical architecture layer...The technical architecture is embedded within the domain parts... [1]

Для микросервисов первично разбиение именно по доменному признаку, естественное разбиение. А техническая архитектура уже [[Encapsulation|инкапсулирована]], а значит **нет причины для coupling между микросервисами на техническом уровне**. Это отражается в независимости выбора языка, фреймворка, архитектурного паттерна.

Полностью избежать *coupling* между микросервисами не получится даже при правильной реализации микросервисов.

> Microservices architectures typically have two kinds of coupling: **integration** and **[[Service template]]**. Integration coupling is obvious — services need to call each other to pass information. [1]

Инкапсуляция логики и данных внутри микросервиса и поддержания контрактов(точек интеграции) ([[Information hiding]]) минимальными позволяет развивать микросервис независимо и следовательно быстро от других.

---

## Источники

1. [[Building evolutionary architectures book]]. Architectural Coupling
1. [[Monolith to microservices book]]

## Ссылки

1. [[Practices]]
1. [[Duplication vs Coupling]]
1. [[Architecture quantum]]
1. [[Microservices prerequisites]]#
1. [База знаний по микросервисам Сергея Баранова](http://agilemindset.ru/%D0%BC%D0%B8%D0%BA%D1%80%D0%BE%D1%81%D0%B5%D1%80%D0%B2%D0%B8%D1%81%D1%8B/)
