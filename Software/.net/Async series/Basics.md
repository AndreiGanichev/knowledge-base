---
date: 2023-11-02
---
# Async. Basics

Конкуррентность(concurrency) - выполнение нескольких действий одновременно

1. Многопоточность - форма *конкуррентности*, использующая *параллелилизм* - большая задача разбивается на более маленькие и каждая из них выполняется одновременно. Параллельная обработка достигается использованием нескольких потоков одновременно.
2. Асинхронность характеризуется **отсутствием блокирования** потоков.

## TPL

> > A task represents a unit of work with a promise to give you results back in the future. That promise could be backed by IO-operation or represent a computation-intensive operation. It doesn't matter. What does matter is that the result of the operation is self-sufficient and is a first-class citizen. You can pass a "future" around: you can store it in a variable, return it from a method, or pass it to another method. You can "join" two "futures" together to form another one, you can wait for results synchronously or you can "await" the result by adding "continuation" to the "future". You can decide what to do if the operation succeeded, faulted or was canceled, just using a task instance alone. [Dissecting the async methods in c# by Sergey Tepliakov](https://blogs.msdn.microsoft.com/seteplia/2017/11/30/dissecting-the-async-methods-in-c/)]

---

## Источники

1. [[Concurrency in C# Cookbook]]

## Ссылки

1. [[Async vs sync]]
1. [[Внутренности асинхронного метода]]
1. [[Antipatterns]]
1. [[Возможности расширения]]
1. [[Издержки асинхронных методов]]
