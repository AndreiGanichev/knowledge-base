---
date: 2022-12-17
tags:
    - tag
---
# Microservices

## Достоинства

> The ability for developers to deploy one service without affecting any other service is one of the defining benefits of this architectural style.

Для того, чтобы новая версия микросервиса работала с текущими версиями других сервисов, необходимо при развитии микросервисов учитывать требования [[Compatibility|совместимости]].

## [[Coupling]]

> Architects often call microservices a **“share nothing”** architecture. The primary advantage of this architecture style is no coupling at the technical architecture layer...The technical architecture is embedded within the domain parts...

Для микросервисов первично разбиение именно по доменному признаку, естественное разбиение. А техническая архитектура уже [[Encapsulation|инкапсулирована]], а значит **нет причины для coupling между микросервисами на техническом уровне**. Это отражается в независимости выбора языка, фреймворка, архитектурного паттерна.

Полностью избежать *coupling* между микросервисами не получится даже при правильной реализации микросервисов.

> Microservices architectures typically have two kinds of coupling: **integration** and **[[Service template]]**. Integration coupling is obvious — services need to call each other to pass information.

Инкапсуляция логики и данных внутри микросервиса и поддержания контрактов(точек интеграции) минимальными позволяет развивать микросервис независимо и следовательно быстро от других.

---

## Источники

1. [[Building evolutionary architectures book]]. Architectural Coupling

## Ссылки

1. [[Practices]]
1. [[Duplication vs Coupling]]
1. [[Architecture quantum]]
1. [[Microservices prerequisites]]#
1. [База знаний по микросервисам Сергея Баранова](http://agilemindset.ru/%D0%BC%D0%B8%D0%BA%D1%80%D0%BE%D1%81%D0%B5%D1%80%D0%B2%D0%B8%D1%81%D1%8B/)
