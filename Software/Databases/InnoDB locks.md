---
date: 2023-11-23
tags:
    - mysql
---
# MySql locks

В MySql есть синтаксис, позволяющий взять блокировку на таблицу целиком, но сама СУБД старается использовать только блокировки конкретных записей(row level lock), это называется *intention lock*.


## Виды

1. *exclusive*(X) - можно называть блокировкой *на запись*: `SELECT ... FOR UPDATE`
1. *shared*(S) можно называть блокировкой *на чтение*: `SELECT ... FOR SHARE`

Если T1 получила X, то T2 при попытке получить блокировку:

1. X будет ждать
1. S будет ждать

Если T1 получила S, то T2 при попытке получить блокировку:

1. X будет ждать
1. S также ее получит и продолжит

### Dead lock

> LOCK IN SHARE MODE is actually often used to bypass multi-versioning and make sure we’re reading most current data, plus to ensure it can’t be changed. This, for example, can be used to read set of the rows, compute new values for **some** of them and write them back. If you want to read set of rows and modify **all** of them you may choose to use SELECT FOR UPDATE. This will ensure you get write locks for all rows at once which reduces the chance of *deadlocks*(см. [InnoDB dead lock example](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlock-example.html)) – lock will not need to be upgraded when an update happens. [Select lock in share mode and for update](https://www.percona.com/blog/select-lock-in-share-mode-and-for-update/)

> Deadlocks affect performance rather than representing a serious error, because InnoDB automatically detects deadlock conditions by default and rolls back one of the affected transactions. [Internal locking](https://dev.mysql.com/doc/refman/8.0/en/internal-locking.html)

> Deadlocks are a classic problem in transactional databases, but they are not dangerous unless they are so frequent that you cannot run certain transactions at all. Normally, you must write your applications so that they are always prepared to re-issue a transaction if it gets rolled back because of a deadlock. [How to Minimize and Handle Deadlocks](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks-handling.html)

### MVCC + locks

MVCC используется для реализации [[ACID Isolation|snapshot isolation]] и позволяет в рамках чтения читать согласовнный слепок данных.

Но блокировки дают возможность даже с таким уровнем изоляции вычитать самые актуальные данные, игнорируя правила MVCC. Это связана с тем, что **блокировка может быть взята только на самом актуальном значении**. Это особенность реализации, которую нужно просто учитывать. Ее можно, например, объяснить тем, что к моменту запроса блокировки запись уже удалена конкурирующей транзакцией, но при этом запись есть в снапшоте, который справедлив для текущей транзакции. Если взятие блокировки было бы основано на снапшоте, то блокировка была бы взята на уже не существующей по факту записи.

## Скоуп

### Record lock

Блокировка берется на *запись* в индексе. Причем это справедливо даже если для таблицы не задано явно индексов.

> row-level locks are actually index-record locks.

```sql
SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE;
```

Этот запрос не дает другим транзакциям вставить, обновить или удалить записи, у которых t.c1 имеет значение 10.

### Gap lock

Блокировка берется на *промежутки* между значениями в индексе.

```sql
SELECT c1 FROM t WHERE c1 BETWEEN 10 and 20 FOR UPDATE;
```

Этот запрос не даст другим транзакциям вставить, например, значение 15 в t.c1. При этом не важно было или не было значение из этого диапазона в столбце, потому что заблокировано все *промежутки* в заданном диапазоне. *Промежуток* может охватывать одно, несколько значений индекса или даже быть пустым.

> their only purpose is to **prevent other transactions from inserting to the gap**

Этим объясняются особенности gap lock по сравнению с record lock:

1. допускается одновременное взятие блокировки на один и тот же диапазон разными транзакциями
1. нет разницы между X и S gap lock
1. gap lock не используется для запросов, которые блокируют используя для этого уникальный индекс для поиска по уникальному значению

Gap locking is not needed for statements that lock rows using a unique index to search for a unique row. For example, if the id column has a unique index, the following statement uses only an index-record lock for the row having id value 100 and it does not matter whether other sessions insert rows in the preceding gap:

```sql
SELECT * FROM child WHERE id = 100;
```

If id is not indexed or has a nonunique index, the statement *does lock* the *preceding gap*. То есть, если в индексе были значения 90, 95, 100, 120, то заблокирован будет промежуток `(95, 100]`

> Gap locks are part of the tradeoff between performance and concurrency, and are used in some transaction isolation levels and not others.

```sql
(negative infinity, 10]
(10, 11]
(11, 13]
(13, 20]
(20, positive infinity)
```

> InnoDB uses next-key locks for searches and index scans, which prevents [[ACID Isolation|phantom]] rows

Допустим в таблице child есть значения с id = 90 и 102.

```sql
SELECT * FROM child WHERE id > 100 FOR UPDATE;
```

Этот запрос возьмет X на записи в индексе со значением больше 100, которая также будет включть gap lock до значения 102, т.е. `(90, 102]`. Если бы этот промежуток не был бы заблокирован от вставки, то параллельная транзакция могла бы вставить значение 101, которое могло бы стать фантомом.

> the next-key locking enables you to “lock” the nonexistence of something in your table

### Когда используются

`READ COMMITTED`

Gap locks не используются для поиска и сканирования индекса. Но все же используется для реализации ограничений (*constraints*), в том числе ограничений внешнего ключа.

`REPEATABLE READ`

Для **nonlocking** reads gap locks не используются(MVCC в этом случае обеспечивает consistent reads).
Для **locking** reads(`SELECT FOR UPDATE/SHARE | DELETE | UPDATE`):

1. Gap locks не используются в уникальном индексе при поиске по уникальному условию. В этом случае используется только record lock
1. В остальных случаях движок СУБД использует gap locks или next-key lock. Тут речь идет именно о блокировке, то есть другая транзакция не сможет вставить в промежуток между значениями индекса. Речь именно о вставке, потому блокируется то именно промежуток - там нет ключей, которые можно было бы удалить или сделать locking read. А неблокируеющее чтение может быть реализовано с помощью MVCC для данного уровня изоляции.

### Next-key lock

Это комбинация record lock + gap lock на предыдущий(до *next key* в индексе) промежуток в индексе.

Suppose that an index contains the values 10, 11, 13, and 20. The possible next-key locks for this index cover the following intervals:

---

## Источники

1. [InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)

## Ссылки

1. [Locks Set by Different SQL Statements in InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-locks-set.html)
1. [[Next-key lock]]
