---
date: 2025-05-27
---
# MySql Performance schema

Это схема, содержащая информацию о работе различных частей MySql.

Основные понятия:

1. instrument - это конкретная метрика, информация о работе какой-то части СУБД. Названия инструмента строятся из сегментов, которые выстраиваются в иерархию, например `statement/sql/select`
2. consumer - это таблица, в которую складываются данные. В основном хранят инструменты в трех вариантах:
    - `_present` - events that are occurring on the server at present
    - `_history` - last 10 completed events per thread
    - `_history_long` - last 10,000 completed events per thread, globally


Генерация и сбор метрик потребляет ресурсы. Для включения/выключения генерации и сбора определенных инструментов существует таблица `setup_instruments`.

## sys schema

Содержит views на основе данных в  `persormance_schema`.

## Анализ запросов

Запросы с какими-либо проблемами можно получить запросом:

```sql
select events_statements_history_long.SQL_TEXT,
       ROWS_EXAMINED,
       ROWS_AFFECTED,
       ERRORS,
       CREATED_TMP_DISK_TABLES,
       CREATED_TMP_TABLES,
       SELECT_FULL_JOIN,
       SELECT_FULL_RANGE_JOIN,
       SELECT_RANGE,
       SELECT_RANGE_CHECK,
       SELECT_SCAN,
       SORT_MERGE_PASSES,
       SORT_RANGE,
       SORT_ROWS,
       SORT_SCAN,
       NO_INDEX_USED,
       NO_GOOD_INDEX_USED
from events_statements_history_long
where ERRORS > 0
   OR CREATED_TMP_DISK_TABLES > 0
   OR CREATED_TMP_TABLES > 0
   OR SELECT_FULL_JOIN > 0
   OR SELECT_FULL_RANGE_JOIN > 0
   OR SELECT_RANGE > 0
   OR SELECT_RANGE_CHECK > 0
   OR SELECT_SCAN > 0
   OR SORT_MERGE_PASSES > 0
   OR SORT_RANGE > 0
   OR SORT_ROWS > 0
   OR SORT_SCAN > 0
   OR NO_INDEX_USED > 0
   OR NO_GOOD_INDEX_USED > 0;
```

## Алгоритм анализа

В документации MySql[1] предлагается действовать итеративно при анализе проблем:

1. воспроизводим проблему
1. анализируем performance_schema
1. выключаем те инструменты, по которым пришло понимание, что нет с ними проблем. Это важный пункт, потому что:

> the Performance Schema output, particularly the `events_waits_history_long` table, contains less and less “noise” caused by nonsignificant instruments, and given that this table has a fixed size, contains more and more data relevant to the analysis of the problem at hand.

4. Далее вносим изменения.

    Once a root cause of performance bottleneck is identified, take the appropriate corrective action, such as:

    1. Tune the server parameters (cache sizes, memory, and so forth).
    1. Tune a query by writing it differently,
    1. Tune the database schema (tables, indexes, and so forth).
    1. Tune the code (this applies to storage engine or server developers only).

5. повторяем заново с п.1

В той же документации описан алгоритм отслеживания блокировок:

1. допустим видим заблокированный тред 1
1. смотрим в `events_waits_current` чего он ожидает
1. допустим ожидает мьютекса А
1. смотрим в `mutex_instances` какой поток взял этот мьютекс, пусть это поток 2
1. снова смотрим в `events_waits_current`, но уже для потока 2


---

## Источники

1. [[High-performance MySql]]

## Ссылки

1. [Using the Performance Schema to Diagnose Problems](https://dev.mysql.com/doc/refman/8.4/en/performance-schema-examples.html)
