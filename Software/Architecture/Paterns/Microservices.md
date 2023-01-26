---
date: 2022-12-17
tags:
    - tag
---

### Достоинства микросервисов

> The ability for developers to deploy one service without affecting any other service is one of the defining benefits of this architectural style.

### Как определить границы микросервиса

> One of the keys to building evolutionary architectures lies in determining **natural component granularity and coupling** bewteen components to fit the capabilities they want to support via the software architecture.

*Natural coupling* определяется предметной областью: то, что меняется вместе, по одним причинам, в одно и то же время. [[Common closure principle]] 

> ...each service is **defined around DDD domain concept**, encapsulating the technical architecture and all other dependent components (like databases) into a bounded context creating a highly decoupled architecture. 

> The goal in microservices isn’t to see how small developers can make each service but rather to create a useful bounded context.

> The physical bounded context in microservices correlates exactly to our concept of architectural quantum — it is a physically decoupled deployable component with high functional cohesion.

> The operational goal of this architecture is to replace one service with another without disrupting other services...The ability for developers to deploy one service without affecting any other service is one of the defining benefits of this architectural style.

### Coupling

> Architects often call microservices a **“share nothing”** architecture. The primary advantage of this architecture style is no coupling at the technical architecture layer...The technical architecture is embedded within the domain parts...

Для микросервисов первично разбиение именно по доменному признаку, естественное разбиение. А техническая архитектура уже инкапсулирована, а значит **нет причины для coupling между микросервисами на техническом уровне**. Это отражается в независимости выбора языка, фреймворка, архитектурного паттерна.


Полностью избежать *coupling* между микросервисами не получится даже при правильной реализации микросервисов.

> Microservices architectures typically have two kinds of coupling: **integration** and **service template**. Integration coupling is obvious — services need to call each other to pass information. The other type of coupling, service templates, prevents harmful duplication...each service needs to include monitoring, logging, authentication/authorization, and other “plumbing” capabilities. If left to the responsibility of each service team, ensuring compliance and lifecycle management like upgrades will likely suffer. By defining the appropriate technical architecture coupling points in service templates, an infrastructure team can manage that coupling while freeing individual service teams from worrying about it.




---

### Источники:
1. [[Building evolutionary architectures book]]. Architectural Coupling

### Ссылки:
1. [[Practices]]
1. [[Duplication vs Coupling]]
1. [[Architecture quantum]]
1. [[Coupling]]
1. [[Microservices prerequisites]]#