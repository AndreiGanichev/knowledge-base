---
date: 2024-02-29
---
# Total order broadcast (TOB)

Это протокол доставки сообщений, который дает гарантии:

1. надежная доставка - сообщения не теряются. При этом не дается гарантий на время доставки.
1. общий порядок - на все получатели сообщения будут доставлены в одном и том же порядке

В отличие от *timestamp ordering*, порядок к моменту доставки сообщения уже определен. В случае *timestamp ordering*  (например [[Lampord clock]] обеспечивает *total order*) можно упорядочить все сообщения, но уже пост фактум, потому что в процессе работы всегда может появиться сообщение, которое должно быть встроено куда-то между уже обработанными сообщениями. В случае использования total order broadcast сообщение может быть *receive* раньше времени, но *deliver* (см. [[Broadcast]])произойдет только когда подойдет его очередь в общем порядке.

Как упоминается [[Ordering|в этой заметке]] существует связь ordering-linearizability.

## Реализац

![Total order broadcast architecture](./Images/Broadcast%20algorithm%20architecture.png)
Источник:[1]

---

## Источники

1. link

## Ссылки

1. #[[Broadcast]]
