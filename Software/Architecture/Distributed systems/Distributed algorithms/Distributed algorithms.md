---
date: 2022-06-16
tags:
    - tag
---

## Свойства распределенных алгоритмов(РА)
1. liveness - событие, предусмотренное РА должно произойти
1. safety - событие, не предусмотренное РА, не должно произойти

### Liveness:
1. в leader election должен быть выбран лидер

### Safety
1. в leader election не должно быть одновременно выбрано два лидера (*split brain*)

---

### Источники:
1. Alex Petrov. Database Internals. Failure detection.

### Ссылки:
1. [[Leader election]]