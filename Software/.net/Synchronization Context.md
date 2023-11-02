---
date: 2023-11-02
---
# Synchronization Context

## Общее

Существуют разные *потоковые модели*, например:

1. Десктопные приложения позволяют обновлять UI только из UI-потока
2. В ASP.NET нет "особых" потоков и все задачи запускаются в потоках из пула

Так вот SC позволяет выполнить делегат в определенной потоковой модели.

> SynchronizationContext is a general abstraction for a “scheduler”.

Но SC не обязательно связан с каким-то конкретным классом планировщика, можно создать свой класс, который будет реализовывать свою логику синхронизации потоков. В примере имеем ограничение на количество запущенных потоков в контексте. Здесь можно сказать и реализовано планирование потоков по правилу, что работают одновременно не более maxConcurrencyLevel.

```csharp
internal sealed class MaxConcurrencySynchronizationContext : SynchronizationContext
{
    private readonly SemaphoreSlim _semaphore;

    public MaxConcurrencySynchronizationContext(int maxConcurrencyLevel) =>
        _semaphore = new SemaphoreSlim(maxConcurrencyLevel);

    public override void Post(SendOrPostCallback d, object state) =>
        _semaphore.WaitAsync().ContinueWith(delegate
        {
            try { d(state); } finally { _semaphore.Release(); }
        }, default, TaskContinuationOptions.None, TaskScheduler.Default);

    public override void Send(SendOrPostCallback d, object state)
    {
        _semaphore.Wait();
        try { d(state); } finally { _semaphore.Release(); }
    }
}
```

Если у нас есть экземпляр SynchronizationContext, то используя его можно поставить в "очередь" на исполнение какой-нибудь делегат. При этом мы не будем знать по каким именно правилам он будет запускаться.

```csharp
    SynchronizationContext sc = SynchronizationContext.Current;
    ThreadPool.QueueUserWorkItem(_ =>
    {
        try { worker(); }
        finally { sc.Post(_ => completion(), null); }
    });
```

Если будет запущено в WPF, то sc.Post выполнит лямбду в UI потоке, несмотря на то, что ранее мы явно использовали пул.

Если мы используем await, то неявно захватывается контекст при вызове асинхронного метода и продолжение будет запланировано в том же контексте. Не в том же потоке, а именно контексте: если десктоп, то в UI потоке, если ASP - отправится в пул.

## ConfigureAwait

Возвращает обертку над Task, но добавляет управление захватом текущего контекста синхронизации:

```csharp
object scheduler = null;
if (continueOnCapturedContext)
{
    scheduler = SynchronizationContext.Current;
    if (scheduler is null && TaskScheduler.Current != TaskScheduler.Default)
    {
        scheduler = TaskScheduler.Current;
    }
}
```

Когда есть смысл указания false:

1. **улучшение перформанса**, потому что вместо простого вызова продолжения в том же потоке, что и основная задача, мы отправляем делегат на исполнение в планировщик
2. **избежание deadlock**. Это наиболее актуально для разработчиков библиотек(general-purpose libraries). Библиотеки должны быть независимы от потоковой модели, которая принята в использующем их приложении.  Допустим есть асинхронный библиотечный метод, внутри которого используется await. И мы синхронно вызовем этот библиотечный метод (.Result или Wait()) в UI-потоке. Библиотечный метод начнет выполняться до первого await. Дальше будет захвачен текущий SC и на нем будет запланировано продолжение метода. Но т.к. текущий контекст предполагает выполнение в UI-потоке, то продолжение метода никогда не сможет выполниться - ведь мы синхронным вызововом заблокировали UI-поток. Получаем deadlock. Причем эта ситуация возможна не только при использовании UI-потока, но и в целом при любом ограничении параллельно выполняемых операций.
При этом false на самом деле не гарантирует того, что не будет использоваться оригинальный контекст. Если задача на момент вызова уже завершена, то продолжение выполняется синхронно в оригинальном потоке.

### Поддержка в .NET Core

Данный метод есть в .NET Core. Что изменилось - это то, что фреймворк больше не устанавливает SC, а значит он никогда не захватывается и нет необходимости вызывать ConfigureAwait(false). При этом можно создать кастомный класс SC, задать его как текущий и в таких случаях уже будет причина вызова ConfigureAwait(false) для задач-продолжений.

---

## Источники

1. [СonfigureAwait FAQ by Stephen Taub](https://devblogs.microsoft.com/dotnet/configureawait-faq/)

## Ссылки

1. link
