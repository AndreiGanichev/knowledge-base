---
date: 2024-02-02
---
# Scalability vs Performance

Чтобы сделать масштабируемую систему, используют репликацию. Но из-за этого появляется несолько копий данных и возникает вопрос синхронизации этих копий и гарантий согласованности, которые предоставляет система. Чем строже выбираем [[Software/Architecture/Distributed systems/Consistency models/Basics|модель согласованности]], тем больше это будет отрицательно влиять на производительность. Например, для обеспечения [[Linearizability]] мы можем выбрать [[Single leader replication]], что ограничит производительность на запись.

---

## Источники

1. link

## Ссылки

1. [[Scalability]]
1. [[Performance]]