---
date: 2024-09-19
---
# Actor model (Orleans)

❗ Статья описывает реализацию акторной модели в фрейморке Orleans. Отличительной особенностью Orleans является реализация **виртуальных акторов**(grains), работа с которыми отличается от работы с акторами в других акторных фреймворках. Чтобы разработчики понимали, что виртуальный актор Orleans ≠ актор в сложившемся понимании, авторы фреймворка назвали виртуальный актор Grain.

## Почему использованы термины Grain и Silo?

> This is analogous to a grains of wheat, as we think of these objects as **small and in adundance**. [5]

> Following the analogy of Grains as tiny seeds, the Silos is the structure that contains them.[5]

## Проблема, которую решает Orleans

Авторы статьи по Orleans [1] так описывают преимущества АМ в современных онлайн приложениях:

> The traditional three-tier architecture(имеется ввиду не трехслойная архитетура бэкенда, а более общий архитектурный паттерн: фронтенд-бэкенд-БД) with stateless front-ends, **stateless** middle tier and a storage layer has limited scalability due to latency and throughput limits of the storage layer that has to be consulted for every request - *data shipping paradigm*.

Эта проблема частично может быть решена с помощью кэширования сущностей в памяти, но в распределенной системе это грозит рассогласованием одной и той же сущности на разных нодах.

Чтобы сделать приложение масштабируемым авторы предлагают использовать акторную модель(*function shipping paradigm*):

> Actors allow building a **stateful** middle tier that has the performance benefits of a cache with data locality and the semantic and consistency benefits of encapsulated entities via application-specific operations.

Statefull акторы, время жизни которых превышает скоуп запроса пользователя, позволяют избавиться от того, чтобы на каждый запрос пользователя делать цикл: запрос сущности из БД -> выполнение бизнес логики -> сохранение в БД изменного состояния сущности. Состояние актора хранится в памяти, а runtime заботиться о времени жизни актора, обеспечивает единственный экзепляр актора в кластере. Гарантии единственного экземпляра актора во всем кластере и последовательной обработки им входящих сообщений обеспечивает однопоточное изменение его состояния во всем кластере. Это отличает использование АМ от кэширования сущностей в памяти.

> rather than fetching the data needed to service a request (from external database or cache), the request is directed to the place where data is held in memory...This has a nice symmetry to the original "big data" thinking, where [[MapReduce|map-reduce]] functions are scheduled on servers holding the data files, rather than loading data into a central data warehouse. [5]

При этом runtime не отвечает за сохранение состояния актора на диск (*durability*), эта ответственность полностью лежит на разработчиках приложения.

## Свойства виртуальных акторов Orleans

1. Perpetual existence - виртуальные акторы(*grain*) Orleans для клиента акторного кластера(самого приложения): приложение просто запрашивает актор нужного типа по ID и получает его. Если нужный актор уже есть, то возвращается *actor reference* на него(.net объект, который является прокси для обращения к актору. Сам актор может быть на другой ноде - см. Location transparency).
1. Automatic instantiation - если запрошенного актора нет, то runtime автоматически создает .net объект, представляющий актор, на одной из машин кластера(зависит от стратегии).
1. Location transparency - при запросе актора код приложения работает с *actor reference* и не знает на какой на самом деле ноде находится актор. Т.к. все коммуникации между акторами полностью асинхронные, то такой способ интеграции обладает всеми преимуществами [[Software/Architecture/Paterns/Event Driven/Basics|event-driven архитектуры]].
1. Automatic scale out - runtime полностью следит за нагрузкой и отвечает за масштабирование. Это особенно актуально для такой разновидности акторов как *stateless worker*, для которых нет требования единственности экземпляра и runtime может создавать их в большом количестве при необходимости.

Как видно runtime забирает на себя бОльшую часть забот о времени жизни актора и позволяет разработчикам приложения сосредоточиться на бизнес логике. Можно провести аналогию с работой с управляемой помятью в .NET с точки зрения обеспечения сборки мусора. Свойства *perpetual existence* и *location transparency* позволяют провести аналогию с работой памяти и ОС, когда часть страниц могут на самом деле быть сохранены на диск и удалены физиески из памяти. Но при необходимости они могут быть восстановлены.

## Виды grains

1. Single activations - это тип актора, который гарантирует только один экземпляр актора **заданного типа и ID** в кластере.
1. Stateless worker - может быть в нескольких экземплярах и масштабироваться runtime'ом.

## Гарантии

1. ❗Eventual Consistency. В случае нормально работы runtime действительно гарантирует единственность актора, НО в случае ошибок(отказ ноды), знания о существующих акторах могут быть потеряны и на какой-то период времени гарантия единственности экземпляра не будет выполняться. Eventualy Orleans обнаружит дубликат и удалит его. Т.е. если обращаться к [[CAP theorem]], то Orleans - это AP система.

> it may be that two activations of the same actor are registered in two different directory partitions, resulting in two activations of a single activation actor. In this case, once the membership has settled, one of the activations is dropped from the directory and a message is sent to its server to deactivate it.[1]

1. Strong isolation - акторы не используют общую память и могут обзаться только путем обмена сообщениями
1. Single-threading - актор обрабатывает сообщения из своего inbox строго последовательно. В совокупности с *single activation* снимает необходимость в распределенных блокировках.
1. Messaging guarantees - по умолчанию Orleans не делает ретраев в случае невозможности доставить сообщение от одного актора другому. Т.о. по умолчанию обеспечивается *at-most- once* гарантия.

В исходной статье [1] так говорилось о гарантия доставки сообщения до актора:

> Orleans provides *at-least-once* message delivery, by resending messages that were not acknowledged after a configurable timeout.

> General wisdom in distributed systems says that maintaining a FIFO order between messages is cheap and highly desirable. Our original implementation followed that pattern, guaranteeing that messages sent from actor A to actor B were delivered to B in order, regardless of failures. This approach however does not scale well in applications with a large number of actors. Moreover, we found that FIFO message ordering is not required for most request-response applications. Developers can easily express logical data and control dependencies in code by a handshake, issuing a next call to an actor only after receiving a reply to the previous call. If the application does not care about the ordering of two calls, it issues them in parallel.

Но в книге [5] написано следующее:

> By default, Orleans is "at most once"...Orleans is configured to never retry message delivery. Retrying could lead to an undesirable outcome, such as a bank balance being credited twice.

Но, сделав обработку сообщений [[Idempotency|идемпотентной]] можно либо в коде приложения ретраить отправку, либо настроить автоматические ретраи в Orleans.

## Что дает акторная модель

![Actor model benefits](../Images/Actor%20model%20benefits.png)
Источник: [4]

Список можно продолжить:
**Promises:** безопасно решения в бизнес логике принимать на основе стейта актора в памяти
**Benefits:** более высокая производительность, потому что не ходим в БД, а работаем с состоянием актора в памяти

**Promises:** общение между акторами полностью асинхронное
**Benefits:** в совокупности с асинхронной моделью .NET(async/await) упрощает код: асинхронное общение между акторами выглядит как синхронное в коде

Один из авторов Orleans Сергей Быков так описывает, что дает разработчикам использование акторного фреймворка:

> the ability to build software using object, grain, that live in virtual "adress spacce" of a cluster of machines as if they were in a single process. [5]

Получается, если .NET CLR - это runtime, обеспечивающий запуск, управление жизнью объектов и адресацию между объектами на одной машине, а Orleans позволяет выйти за пределы одной машины и предоставляет runtime, который грубо говоря управляет теми же .NET объектами, но расположенными на разных машинах кластера. Поэтому одним из достоинств Orleans называют упрощение программной модели, потому что разработчики пишут код, который они всегда писли для выполнения на одной машине, а он запускается на целом кластере.

> Orleans lifts a few concepts out of the database, such a transactions, which allows you to use an underlying data store with fairly limited set of guarantees, but provides [[ACID]] compliance at the application level.[5]

### Когда акторная модель подходит [2]

1. Your problem space involves concurrency. Without actors, you’d have to introduce explicit locking mechanisms in your code.
1. Your problem space can be partitioned into small, independent, and isolated units of state and logic.
1. You don’t need low-latency reads of the actor state. Low-latency reads cannot be guaranteed because actor operations execute serially. (прим. Orleans вроде позволяет выполнять запросы на чтение вне очереди и регулирует это спец атрибутами)
1. You don’t need to query state across a set of actors. Querying across actors is inefficient because each actor’s state needs to be read individually and can introduce unpredictable latencies.

### Когда акторная модель НЕ подходит [1]

1. application that intermixes frequent bulk operations on many entities with operations on individual entities. Isolation of actors makes such bulk operations more expensive than operations on shared memory data structures
1. virtual actor model can degrade if the number of actors in the system is extremely large (billions) and there is no temporal locality
1. Orleans does not yet support crossactor transactions, so applications that require this feature outside of the database system are not suitable

## Actor and Saga

[[Saga pattern]], а точнее **orchestration** saga, может быть реализован с использованием акторов. Актор в данном случае как раз координирует выполнение шагов саги. Примеры:

1. Dr. Roland Kuhn, один из авторов книги Reactive Design Patterns в своем докладе [3] описывает реализацию саги на акторах Akka
1. В референсном приложении от Microsoft [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) процесс заказа реализован как сага на основе акторов([2] глава Dapr reference application параграф Actors). Более в [2] после описания use cases, для которых подходят акторы, написано:

> One design pattern that fits these criteria quite well is the orchestration-based saga or process manager design pattern.

---

## Источники

1. [Orleans: Distributed Virtual Actors for Programmability and Scalability](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/Orleans-MSR-TR-2014-41.pdf)
1. [Dapr for NET Developers book](https://github.com/dotnet-architecture/eBooks/blob/1ed30275281b9060964fcb2a4c363fe7797fe3f3/current/dapr-for-net-developers/Dapr-for-NET-Developers.pdf)
1. [Dr. Roland Kuhn. Reactive design patterns](https://youtu.be/Rm8n-H6zI1k?si=FMT4GoN7ZzvFvUi_&t=2944)
1. [Aaron Stannard — When and how to use the actor model: An introduction to Akka.NET actors](https://youtu.be/MY1iPY78_fs?si=AXgvGi0vGdH25H4L&t=4272)
1. Microsoft Orleans for Developers: Build Cloud-Native, High-Scale, Distributed Systems in .NET Using Orleans. Richard Astbury.

## Ссылки

1. link
