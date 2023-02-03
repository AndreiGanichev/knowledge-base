---
date: 2023-01-27
tags:
    - tag
---

### Как определить границы микросервиса

> One of the keys to building evolutionary architectures lies in determining **natural component granularity and coupling** bewteen components to fit the capabilities they want to support via the software architecture.

*Natural coupling* определяется предметной областью: то, что меняется вместе, по одним причинам, в одно и то же время. [[Common closure principle]] 

> ...each service is **defined around DDD domain concept**, encapsulating the technical architecture and all other dependent components (like databases) into a bounded context creating a highly decoupled architecture. 

> The goal in microservices isn’t to see how small developers can make each service but rather to create a useful bounded context.

> The physical bounded context in microservices correlates exactly to our concept of architectural quantum — it is a physically decoupled deployable component with high functional cohesion.

> The operational goal of this architecture is to replace one service with another without disrupting other services...The ability for developers to deploy one service without affecting any other service is one of the defining benefits of this architectural style.

---

### Источники:
1. [[Building evolutionary architectures book]]. Architectural Coupling

### Ссылки:
1. [[Modularity]]
1. [[Bounded context]]