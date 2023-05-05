---
date: 2023-05-05
---
# Appropriate coupling

> As in physics, four fundamental interactions exist in nature: gravitational, electromagnetic, strong, and weak. The strong nuclear force, which holds atoms (and therefore ordinary matter) together, is notable for its strength. Breaking it unleashes much of the power of nuclear fission. Similarly, some architectural components are extremely difficult to break into smaller pieces. Metaphorically, they exhibit strong nuclear force. One of the keys to building evolutionary architectures lies in determining **natural component granularity** and coupling bewteen components to fit the capabilities they want to support via the software architecture.

Понятие appropriate coupling применимо на разных уровнях абстракции. На уровне сервисов оно связано с [[Architecture quantum]]. А внутри сервиса определяет внутреннюю архитектуру. [[Layered architecture]] основана на формировании слоев по техническому признаку. [[Modular monolith]] предполагает объединение в модуля по функциональному признаку.

В [[Layered architecture]] разбиение сделано по техническому признаку(см. [[Architecture dimensions]]). Это *intentional* coupling. Но знания о предметной области оказываются распределены между слоями, что создает *unintentional* coupling. То есть естественные силы/естественная гранулярность связывает их(поэтому их приходится менять вместе), несмотря на то, что их разделили по слоям.

> one of the important aspects of an evolvable architecture is appropriate coupling across dimensions.

Это значит, что после того как определили важные в конкретном проекте *architecture dimensions*, необходимо проектировать архитектуру так, чтобы изменения в этих измерениях были легкими.

> First make the change easy, then make the easy change. Kent Beck

При этом ничего не мешает использовать слоеную архитектуру внутри микросервиса, просто она будет инкапсулирована внутри физических границ сервиса и не будет никак связана с технической архитектурой других сервисов.

Разделив систему на [[Microservices]] в соответствии с предметной областью, мы получаем легкость изменений в "доменном" измерении архитектуры, но получаем проблему в техническом и операционном измерениях: создается возможность зоопарка решений однотипных инфраструктурных задач в разных микросервисах. Это противоречит принципу [[Remove needless variability]] и может быть исправлено с помощью [[Service template]]. **Service template - пример appropriate coupling**.

---

## Источники

1. link

## Ссылки

1. #[[Coupling]]
