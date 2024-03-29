---
date: 2022-01-21
tags:
    - tag
---
# Architecture quantum

Архитектурный квант - это минимальная единица деплоя, обеспечивающая полноценную часть функционала системы. Охватывает не только сам сервис, но и все необходимое для его полноценной работы:

1. БД
1. вспомогательные сервисы, такие как поисковые движки или отчеты

Другими словами - это часть системы, в которой скорее всего придется сделать изменения, когда добавляется/изменяется какой-то функционал.

В монолитном приложении квант - вся система, потому что это и есть единица деплоя. Распределенный монолит - пример того, что несмотря на вроде бы распределенную природу микросервисов, все равно при плохом проектировании можно получить квант размером со всю систему.

Чем меньше квант, тем лучше, потому что проще понимать, легче деплоить и значит обеспечивает *incremental change* характеристику эволюционной архитектуры.

> The quantum size of an architecture largely determines how easy it will be for developers to make evolutionary changes.

## Arch quantum vs Cohesion

> Architecture quantum differs from traditional thinking about [[Cohesion]] by encompassing dependent components like databases.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. #[[Evolutionary architecture]]
1. [[Modularity]]
1. [[Microservice size]]
