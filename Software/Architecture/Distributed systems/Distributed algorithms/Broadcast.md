---
date: 2024-02-29
---
# Broadast

Это базовый распределенный алгоритм, описывает доставку сообщений от одной ноды нескольким другим.

Когда речь идет о broadcast алгоритмах, то выделяют отдельный middleware, который реализует алгоритм и ноды коммуницируют через него. Плюс разделяют факт *receive* и *deliver*. Receive - это когда по сети сообщение дошло от middleware на одной ноде, до middleware на другой. В этом моменте сообщение может быть буферизовано или положено во внутреннюю очередь, потому что согласно алгоритму его еще рано доставлять до получателя. А вот когда приходит очередь сообщения происходит deliver до получателя.

![Total order broadcast architecture](./Images/Broadast%20algorithm%20architecture.png)
Источник:[1]

## Best effort

Отправитель делает все возможное для отправки, но в случае отказа отправителя в процессе отправки сообщение дойдет не до всех получателей. Т.о. доставка не гарантирована.

## Reliable

Гарантирует доставку, но не гарантирует порядок. Можно реализовать на основе *best effort*, но те ноды которые получили сообщение могут сами пытаться ретранслировать другим нодам. Например может исопльзоваться [[Gossip dissemination]].

## Reliable + order

Возможны разные реализации в зависимости от гарантий порядка:

1. FIFO - сообщения с ноды-отправителя доставляются в том порядке, в котором отправлены.
1. Casual - порядок основан на [[Happens-before relationship]].
1. [[Total order broadcast]]

## Когда какой использовать?

Естественно, что чем больше гарантий дает реализация broadcast'a, тем медленнее он будет работать. *Total order broadcast* не всегда нужен и можно получить желаемые требования с более слабым алгоритмом:

1. **casual** broadcast при условии, что сообщения, не связанные отношением happens-before, [[Commutative|коммутитивные]]. Возможно сюда подходит пример с кафка: порядок доставки гарантируется только в рамках партиции. И уже задача application опеделить какие сообщения связаны отношением happens before и дать им один ключ партиционирования. И в целом Клепман при объяснении работы TOB говорит: "Another way of looking at total order broadcast is that it is a way of creating a *log*: delivering a message is like appending to the log. Since all nodes must deliver the same messages in the same order, all nodes can read the log and see the same sequence of messages."
1. **reliable** при условии что все сообщения коммутативные.
1. **best effort** при условии, что все операции коммутативные, обработчики обеспечивают [[Idempotency|идемпотентность]] и терпимы к потерям.

---

## Источники

1. [Distributed systems lecture series by Martin Kleppmann. Distributed Systems 4.2: Broadcast ordering](https://youtu.be/A8oamrHf_cQ?si=ww9HoF3odKuHHUGA)
1. [[Designing Data-Intensive Applications book]]. Chapter 9. Total order broadcast.

## Ссылки

1. link
