---
date: 2022-09-08
tags:
    - tag
---

Доменная модель - это объектная модель домена, содержащая данные и бизнес-логику

### Цели паттерна
1. борьба со сложностью: бизнес логика сложных доменов и так очень сложная, потому паттерн предлагает очистить доменную модель от всего кроме этой бизнес логики. Т.е. убрать любое использование инфраструктуры и фреймворков. Объекты доменной модели должны быть *POCO*.
1. выражение мыслей в коде на [[Ubiquotous Language]]

### Строительные блоки доменной модели

1. [[Value object]]
1. [[Aggregate]]
1. [[Domain event]]
1. [[Domain service]]

---

### Источники:
1. Learning DDD. Chapter 6. Tackling complex business logic. Vladik Khononov

### Ссылки:
1. Martin Fowler. Domain model article
1. POCO
