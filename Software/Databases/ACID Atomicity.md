---
date: 2024-03-08
---
# ACID Atomicity

## Термин атомарность

*Атомарность* операции с точки зрения примитивов синхронизации языков программирования имеет значение в контексте конкуренции: операция **неделима**, т.е. другие операции не могут "вклиниться" в выполнение атомарной операции.

В контексте ACID атомарность - это гарантия "все или ничего": операция либо будет выполнена полностью, либо совсем никак. Т.о. СУБД гарантирует, что в случае отката транзакции от ее выполнения не останется никаких следов. Такой подход существенно упрощает жизнь разработчикам приложений, которые используют СУБД. Например, это позволяет смело делать ретраи и автоматические ретраи вообще могут быть встроены на уровне ORM.

Атомарность особенно актуальна в случае если в транзакции модифицируются разные объекты(multi-object). Например, разные таблицы. Но это также относится к вторичным индексам, потому что с точки зрения движка СУБД вторичные индексы - это отдельные первичных данных структуры.

## Реализация

В случае работы СУБД на одной ноде **атомарость реализуется на аппаратном уровне**. Операции в рамках транзакции фиксируются в [[Write ahead log]] с указанием ID транзакции в рамках которой они выполняются. Когда поступает команда на выполнения коммита, то она тоже пытается записаться в WAL. После того как драйвер диска ответил, что команда записана в лог, транзакция уже не может быть отменена. В случае падения СУБД состояние данных будет восстановлено из WAL с учетом закомиченных транзакций.

> key deciding moment for whether the transaction commits or aborts is the moment at which the disk finishes writing the commit record: before that moment, it is still possible to abort (due to a crash), but after that moment, the transaction is committed (even if the database crashes). Thus, it is a single device (the controller of one particular disk drive, attached to one particular node) that makes the commit atomic. [2]

## Распределенные транзакции

Алгоритм обеспечения атомарности в распределенных системах называется *atomic commit*. Примером его реализации является [[Two-phase commit]].

---

## Источники

1. [[Designing Data-Intensive Applications book]]. Chapters 7.
1. [[Designing Data-Intensive Applications book]]. Chapters 9.

## Ссылки

1. #[[ACID]]
