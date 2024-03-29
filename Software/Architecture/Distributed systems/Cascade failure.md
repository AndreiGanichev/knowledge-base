---
date: 2022-06-08
---

# Cascade failure

Возникает в распределенной системе, когда из-за отказа (или недоступности из-за проблем с сетью) одного процесса нагрузка перекладывается на другие процессы. Ситуация усугубляется тем, что клиенты, которым пришла ошибка из-за отказа начинают повторять запросы, тем самым еще больше увеличивая нагрузку. В результате чего другие процессы также не выдерживают и отказывают. В итоге чем больше процессов отказало, тем выше нагрузка на оставшиеся рабочие процессы и выше вероятность их отказа. В итоге система может стать отказать в целом.

Ожидание ответа от процесса может также провоцировать каскадное увеличение нагрузки. Если процессы в распределенной системе вызывают друг друга по цепочке и процесс в конце цепочки упал или сильно замедлился, то вся цепочка будет ждать ответа от этого процесса(не факт что в итоге дождется). *Timeout pattern* не решает эту проблему, потому что в любом случае есть время ожидания в течение которого:

1. занято сетевое соединение
1. заняты потоки на нодах
1. возможно на каких-то нода открыты транзакции

Все это приводит к **горизонтальному** распространению нагрузки. Кроме этого чем дольше медленный процесс обрабатывает запросы, тем больше становится параллельных запросов, потому что частота поступления запросов начинает превышать частоту ответов. В итоге получаем **вертикальное** увеличение нагрузки на нагруженной ноде. В итоге она отказывает.

Подсистема *failure detection* помогает исключить ожидание распределенным алгоритмом отказавшей ноды.

На практике сталкивался с проблемой реализации health check(см. [[Software/Ops/Basics]]. Health check), когда при большой нагрузке все потоки были заняты и не было свободных потоков для обработки запроса health check, в результате чего k8s начал подозревать инстанс сервиса в отказе и в итоге перезапускал. Перезапуск инстанса требовал времени, приводил к возврату сообщений от останавливаемого сервиса в очередь сообщений. Короче в итоге приводил к усугублению проблем с нагрузкой.

## Как решить

1. *Rate limit* - поможет ограничить поток запросов
1. *Bulkhead pattern* - поможет избежать *noisy neighbor* проблемы
1. *Circuit breaker* - следуя принципу *fail fast* дает возможность серверу восстановиться
1. *Timeout* на запросы. В одиночку не решит проблему, но и без него никак
1. использовать асинхронное взаимодействие вместо синхронного. Тогда процесс будет обрабатывать сообщения/запросы настолько быстро насколько он может
1. *Kill switch* - это как аварийная кнопка на случай высококй нагрузки для отключения некритичных операций или переключения синхронного взаимодействия на асинхронное
1. ограничить объем данных, которые могут быть запрошены у сервера
1. валидация данных на клиенте. Это позволит не нагружать сеть и сервер запросом, который все равно будет отвергнут

---

## Источники

1.[Википедия](https://en.wikipedia.org/wiki/Cascading_failure#In_computer_networks)

## Ссылки

1. [[Load parameters]]
1. [[Circuit breaker pattern]]
1. [[Retry pattern]]
1. [[Timeout pattern]]
1. [[Rate limit pattern]]
1. [[Failure detection]]
1. [[Bulkhead pattern]]
1. [[Latency model]]
1. [Noisy neighbor](https://docs.microsoft.com/en-us/azure/architecture/antipatterns/noisy-neighbor/noisy-neighbor)
