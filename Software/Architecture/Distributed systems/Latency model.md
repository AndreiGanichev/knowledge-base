---
date: 2023-11-02
---
# Latency model

> Асинхронность(общее) - не совпадение с чем-либо во времени; неодновременность, несинхронность — характеризует процессы, не совпадающие во времени. В компьютерном программировании, асинхронными событиями являются те, которые возникают независимо от основного потока выполнения программы. Асинхронные действия — действия, выполненные в неблокирующем режиме, что позволяет основному потоку программы продолжить обработку. [Wikipedia](https://ru.wikipedia.org/wiki/%D0%90%D1%81%D0%B8%D0%BD%D1%85%D1%80%D0%BE%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D1%8C)

## Synchronous

Предполагает наличие *notion of time*:

1. bounded network delay
1. bounded process pauses
1. bounded clock drift
1. скорость работы процессов сопоставима

Это не значит, что задержек нет или часы абсолютно синхронны. Это значит, что **известно максимальное значение задержек** и отклонения часов.

> The synchronous model is not a realistic model of most practical systems, because unbounded delays and pauses do occur.

### Причины сетевых задержек

Задержки при передачи пакета по сети возникают из-за наличия очередей на разных этапах передачи пакета:

1. if several different nodes simultaneously try to send packets to the same destina‐ tion, the network switch must queue them up
1. incoming request is queued by the operating system
1. in virtualized environments incoming data is queued (buffered) by the virtual machine monitor while another virtual machine uses a CPU core
1. TCP performs flow control (also known as congestion avoidance or backpressure). This means additional queueing at the sender before the data even enters the network
1. TCP (в отличии от UDP) снова посылает пакет, если он потерялся. Это создает дополнительную нагрузку на сеть.

### Причины остановки процессов

Самая очевидная причина - garbage collection. В этом случае как минимум происходит замедление 

## Partially synchronous



## Asynchronous

---

## Источники

1. [[Designing Data-Intensive Applications book ]]. Chapter 8.

## Ссылки

1. [Synchrony, Asynchrony and Partial synchrony](https://decentralizedthoughts.github.io/2019-06-01-2019-5-31-models/)
