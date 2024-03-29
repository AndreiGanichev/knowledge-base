---
date: 2023-08-08
---
# Replication

Репликация - это пример [[Horizontal scaling]] и ее суть проста - хранение нескольких копий данных на разных нодах кластера.

Это позволяет:

1. повысить [[Availability]] и *fault taulerance*: в случае недоступности/падения одной из нод чтение можно производить с других реплик
1. повысить [[Scalability]]: нагрузку на чтение можно распределять по всем репликам
1. снизить [[Latency]]: читать данные с той реплики, которая физически ближе к клиенту(*георепликация*)

## Основная проблема реализации репликации

Проблемой является то, что как только мы добавляем копии данных, то система в целом становится распределенной и возникает **проблема синхронизации копий данных**. Откуда следую разные гарантии и [[Software/Architecture/Distributed systems/Consistency models/Basics|consistency models]].

Репликация отлично работает, если данные после записи не обновляются, например [Content Delivery Network](https://ru.wikipedia.org/wiki/Content_Delivery_Network). Но в случае, если есть обновления, возникает проблемы с согласованностью данных. И тут имеем классический trade-off: чем больше хотим доступности и масштабируемости, тем больше должны ослаблять требования согласованности.

## Виды репликации [[Latency model]]

1. синхронная - запись считается успешной только если в ходе того же запроса от пользователя данные были реплицированы
1. асинхронная - производится в фоне, после того как на запрос пользователя ответили успехом. Неизбежны проблемы с отставанием реплик - *replication lag*. Следствием этого будут возникать проблемы с согласованностью данных.

> In practice, if you enable syn‐ chronous replication on a database, it usually means that one of the followers is syn‐ chronous, and the others are asynchronous. This configuration is sometimes also called **semi-synchronous**.

## Способы репликации

1. statement replication. Для реплицирования данных передаются прямо запросы, которые были выполнены на лидере(или той реплике, на которую попала запись в случае *leaderless*). Достоинства: простота. Недостаток: запросы могут быть завязаны на состояние или контекст исполнения(dateTime.Now, autoincrement), могут инициировать работу триггеров или хранимых процедур. Короче говоря результат выполнения этого запроса на репликах будет иной.
1. передача [[Write ahead log]]. Достоинство - простота(СУБД в любом случае пишет WAL). Недостаток: формат WAL является низкоуровневой деталью реализации СУБД и может меняться от версии к версии, не говоря уже о совместимости WAL для разных СУБД(на случай если данные реплицируются в другую СУБД). Т.о. на фоловере должно хранилище совпадать вплоть до версии, что может делать невозможным обновление СУБД без полной остановки кластера.
1. logical or row-based replication. Идея похожая на передачу WAL - передается что в итоге было сделано, но только операции описываются на более высоком уровне абстракции и не привязаны к конкретному формату WAL. Это отвязывает фоловеров от лидера. Чаще всего реплицируемые операции передаются по каждой записи(row).

## Реализация

- [[Single leader replication]]#
- [[Multi leader replication]]#
- [[Leaderless replication]]#

## Replication lag

Это отставание реплики от лидера. Это может вызывать нарушение таких гарантий согласованности как:

1. read-after-write
1. monotonic reada
1. consistent prefix reads

---

## Источники

1. [[Designing Data-Intensive Applications book ]]. Chapter 5.

## Ссылки

1. [[Software/Architecture/Distributed systems/Consistency models/Basics]]
1. [[Cluster metadata replication]]
