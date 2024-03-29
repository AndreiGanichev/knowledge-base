---
date: 2022-05-28
tags:
    - kafka
    - zookeeper
    - replication
    - memory-vs-disk
---

# Kafka design

## Хранение

В отличие от других систем Kafka использует хранение на диске, а не в памяти. Это обусловлено предположением, что последовательный доступ даже к магнитным дискам очень быстрый, может быть даже быстрее, чем random access в памяти. Поэтому Kafka для хранения использует возможности ОС, которая также закрывает вопрос с кэшированием. Кроме того использование объектов в памяти - это overhead с точки зрения занимаемого объема - бинарные данные занимают меньше места. Кроме того отсутствуют недостатки GC в JVM. Короче по полной используется файловая система и в отличие от более традиционного подхода наоборот все стараются в первую очередь записать в файловую систему(при этом она не обязательно сразу флашит на диск), а память используют по остаточному признаку. Кроме того даже если Kafka перезапускается кэш ОС все равно остается и его после перезапуска не нужно прогревать.

Брокеры сообщений обычно хранят сообщения в очереди, которая создается на каждого consumer'a. И для произвольного доступа добавляется индекс в виде B-tree. Но B-tree плохо работают для диска. Поэтому в Kafka используется log-structure с возможностью читать большие куски и писать append-only. При этом записи не блокируют и могут идти параллельно к чтению(не как в B-tree). А для чтения нужно только один раз переместить головку на офсет и потом читается последовательно большой кусок. Это дает новые возможности, например хранить сообщения столько, сколько нужно.

Ссылки:

1. [[LSM tree]]
2. [[B-tree]]

## Эффективность

Kafka поставила целью максимальную эффективность, чтобы приложения, использующие ее, первыми не выдерживали нагрузку, а она могла с такой справляться. Короче чтобы инфраструктурный сервис не был никогда узким местом.

Использование диска(помимо random seek паттерна доступа, который не используется в Kafkа) может снижать эффективность:

1. **много маленьких IO**. Эта проблема решается батчингом событий, применение которого на всем пути сообщения дает многократное улучшение на всех уровнях: меньше походов по сети, меньше IO, использование последовательных блоков памяти.
1. **многократное копирование байтов**

> To understand the impact of sendfile, it is important to understand the common data path for transfer of data from file to socket:
1)The operating system reads data from the disk into pagecache in kernel space
2)The application reads the data from kernel space into a user-space buffer
3)The application writes the data back into kernel space into a socket buffer
4)The operating system copies the data from the socket buffer to the NIC buffer where it is sent over the network

Kafka использует один и тот же бинарный формат для хрананения, консьюмеров и продьюсеров. Поэтому для передачи сообщения надо просто взять его из файловой системы и отправить как есть в сокет. Linux имеет для этого отдельную системную функцию *sendfile* и только ее достаточно вызвать.

Для снижения вероятности забивания канала сети Kafka предлагает сжатие. Причем очень эффективное, потому что сжимается батч сообщений целиком. Это дает возможность увеличить компрессию, потому что сообщения могут иметь повторяющиеся поля(имена полей в json или общие строки и т.п.), которые могут быть переданы только в одном экземпляре в батче. Сжатие используется не только для передачи по сети, но и хранения. Недостатком при сжатии целого батча является то, что отправить также можно только его целиком.

## Produce

Клиент буферизирует публикуемые сообщения и передает(push) их на сервер пачкой. 

## Consume

Используется *pull-модель* - консьюмер сам запрашивает сообщения и передает *offset*, начиная с которого ему нужны сообщения. 

Чтобы сократить походы к брокеру в случае длительного отсутствия сообщений, используется *long-polling*.

Брокеры обычно используют схему с ожиданием *ack* от consumer’a, чтобы понять, что сообщение прочитано. Но это добавляет больше возможных состояний (received->sent->acked) и соответственно усложняет логику брокера. В Kafka все проще - хранится только указатель(int) на текущий *offset* в логе для каждого консьюмера.

Ссылки:

1. [[Pull vs Push]]

## Гарантии доставки

### Публикация

Брокер присваивает каждому паблишеру ID и ведет счетчик опубликованных сообщений, чтобы обеспечить дедупликацию и *exactly once* **публикацию** сообщения. Повторная отправка с клиента может понадобится даже если брокер на самом деле закомитил сообщение, но вот ответ не дошел до клиента.  Но можно настроить и так, что будет *at most once* публикация, когда клиент не будет ждать коммита, а просто отправит сообщение. Т.е. в теории оно может не закомититься в лог.

Kafka поддерживает публикацию в несколько партиций топика в транзакционном стиле.

### Потребление

Факт потребления отмечается сдвигание офсета:

1. если сначала передвинуть офсет, а потом взять в обработку, то получим at-most-once
1. если сначала обработать, а потом сдвинуть, то at-least-once

В Kafka Streams используется подход, когда результат обработки сообщения перекладывается в другой топик. Так вот в одной транзакции вместе с публикацией в этот следующий топик записывается и offset по итогам обработки сообщения
В упомянутых выше транзакциях как обычно есть *isolation level*, который определяет что доступно consumer’ам: read committed, read uncommited

## Репликация

Настройки репликации конфигурируются на каждый топик, единица репликации - это партиция. Для репликации используется схема leader-followers. Вся запись идет на лидера, чтение может идти с лидера и фоловеров(это обновили в документации буквально в момент ее чтения. До этого чтение все тоже были с лидера). Фоловеры затягивают(pull) обновления с лидера как обычные консьюмеры и append’ят новые сообщения в конец лога. Лидер ведет список **in-sync replicas (ISR)**. Сообщение продьюсера считается закомиченным, если оно добавлено в лог на **все**(плюс есть настройка на топик: какое может быть минимальное кол-во таких реплик для того, чтобы была доступна публикация) ISR. Консьюмер видит только закомиченные сообщения. Можно настроить так, что продьюсер будет ждать сохранения только на лидера(минимальное кол-во ISR тогда равно 1, т.е. сам лидер) и не ждать дальнейшей репликации. Это трейдоф: latency vs durability.

Реплика считается in-sync, если:

1. она отвечает на heartbeats: *"A node must be able to maintain its session with ZooKeeper (via ZooKeeper's heartbeat mechanism)"*.
1. не слишком отстала от лидера(отставание конфигурируется *replica.lag.time.max.ms*)

Zookeeper используется для хранения метаинформации о кластере Kafka(ноды, топики, партиции). Каждый брокер(нода кластера) подключается к Zookeeper и передает ему свои данные, при этом получает информацию о конфигурации всего кластера.

Кафка использует crash-recovery failure model. При этом Kafka не полагается на то, что после recovery нода имеет на поврежденные данные. Во-первых проблемы с диском - это наиболее частая причина падений, во-вторых такое решение принято, чтобы можно было более агрессивно использовать кэш ОС без флаша на диск.

> Kafka will remain available in the presence of node failures after a short fail-over period, but may not remain available in the presence of network partitions. 

Получается Kafka выбирает *CP* (consistency + partition tolerance) подход.Также такому поведению соответствует решение что делать при падении вообще всех реплик. Можно было бы дождаться восстановления любой ноды и снизить downtime(повысить availability), но Kafka будет ждать пока поднимется нода из ISR, чтобы обеспечить consistency.

Ссылки:

1. [[Failure models]]
1. [[CAP theorem]]

Чтобы реплицировать log(последовательность записей) можно использовать **replicated log** алгоритм. Это как примитив для построения распределенных систем. Он позволяет синхронизировать копии лога на нескольких машинах.

Ссылки:

1. [Replicated log](https://martinfowler.com/articles/patterns-of-distributed-systems/replicated-log.htm)
1. [[Cluster metadata replication]]

Партиций в кластере большое кол-во. Они распределяются равномерно по брокерам(нодам). При этом, если нода выходит из строя, то скорее всего на ней было несколько лидеров партиций. Чтобы избежать дублирования служебных операций при выполнении leader election отдельно для каждой партиции, в кластере назначается **controller**, который управляет выборами для всех партиций и делает батчинг однотипных операций в ходе выборов в разных партициях.

### Где в Kafka используется консенсус?

Консенсус в Kafka - это консенсус реплик партиции топика Kafka о текущем наборе ISR. Этот консенсус обеспечивается Zookeeper, который реализует atomic broadcast протокол для репликации уже внутри Zookeeper.

> Because all replicas agree (via ZooKeeper) on what the latest ISR is, there is no possibility of split brain scenarios.

## Log compaction

Хранить полную версию лога долгое время может быть невыгодно с точки зрения занимаемого места. Поэтому длина лога(retention) может регулироваться:

1. по времени
1. по объему
1. использованием compaction

Log compaction может быть необходим, когда хочется сократить хранимый лог, но при этом сохранить по каждому идентификатору(сущности к которой относятся события) самое последнее сообщение. Тогда мы потерям промежуточные изменения состояний, но наиболее актуальное все равно будем иметь. При этом все записи, которые пережили compaction, сохраняют свои изначальные офсеты.

У Kafka есть настройки времени для записи раньше которого ее нельзя компактить и времени после которого запись должны будет закомпакчена.
Отдельная история с записями об удалении значения по ключу(*tombestones*). У них свой цикл компакшена - время после которого их можно удалять настраивается отдельно.
Compaction производится в фоновом процессе и не должен мешать чтению консьюмеров. Плюс можно ограничить ресурсы, которые потребляет этот процесс.

---

## Источники

1. [Distributed Consensus Reloaded: Apache ZooKeeper and Replication in Apache Kafka](https://www.confluent.io/blog/distributed-consensus-reloaded-apache-zookeeper-and-replication-in-kafka)
1. [Kafka documentation](https://kafka.apache.org/documentation/#design)

### Ссылки

1. #[[Kafka basics]]
