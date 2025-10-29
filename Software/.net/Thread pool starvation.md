---
date: 2025-02-19
---
# Thread pool starvation

> ThreadPool starvation occurs when there are no free threads to handle the queued work items and the runtime responds by increasing the number of ThreadPool threads. The dotnet.thread_pool.thread.count value increases rapidly to 2-3x the number of processor cores on your machine, and then further threads are added 1-2 per second until stabilizing somewhere above 125. The key signals that ThreadPool starvation is currently a performance bottleneck are the slow and steady increase of ThreadPool threads and CPU Usage much less than 100%. The thread count increase will continue until either the pool hits the maximum number of threads, enough threads have been created to satisfy all the incoming work items, or the CPU has been saturated. Often, but not always, ThreadPool starvation will also show large values for dotnet.thread_pool.queue.length and low values for dotnet.thread_pool.work_item.count, meaning that there's a large amount of pending work and little work being completed. [2]

---

## Источники

1. [Diagnosing .NET Core ThreadPool Starvation with PerfView](https://learn.microsoft.com/fr-fr/archive/blogs/vancem/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall)
1. [Debug ThreadPool starvation](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/debug-threadpool-starvation)

## Ссылки

1. [[Threads, TPL]]
