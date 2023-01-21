---
date: 2022-07-30
tags:
    - tag
---

Event driven подход при построении архитектуры хорош, когда разработчики погружены в домен. Эта погруженность позволяет им легко ориентироваться в происходящих в системе доменных событиях. Если разработчик понимает какое событие ему нужно, то быстро переходит к его обработчику и вдит нужную логику.

И наоборот, когда события не используются, то в таком коде проще разбираться, если нет погружения в домен - просто идешь по цепочки вызовов и явно видишь, что происходит.

Если разработчик плохо разбирается в домене, то сходу читать код, построенный на событиях ему тяжело, потому что события как асинхронный тип взаимодействия, разрывают связанность и становится сложно просто пройти от начала до конца сценария.


---

### Источники:
1. Telegram канал Ивана Закревского

### Ссылки:
1. #[[Domain event]]