---
date: 2022-07-19
tags:
    - dotnet
    - tracing
---

В dotnet исторически свой подход к работе с сигналами - пр-во имен ```System.Diagnostics```

Activity используется только для хранения контекста трассировк и его передачи по трейсу. Для записи ```Activity``` и сбора событий в рамках ```Activity``` используются ```ActivityListener``` и ```DiagnosticListener```. Если для ```ActivitySource``` не добавлено слушателей, то он не будет инстанциировать ```Activity```, чтобы не расходовать ресурсы. Поэтому надо быть готовым, что при создании Activity она всегда может оказаться ```null``` в runtime.

### Реализация OpenTelemetry

*Span = Activity*
*Tracer = ActivitySource*

---

### Источники:
1. [Microsoft docs](https://docs.microsoft.com/en-us/dotnet/core/diagnostics/)

### Ссылки:
1. #[[OpenTelemetry]]
1. [DiagnosticSource Users Guide](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)
1. [Building End-to-End Diagnostics and Tracing by Jimmy Bogard](https://jimmybogard.com/building-end-to-end-diagnostics-and-tracing-a-primer/)
1. [Introducing experimental OpenTelemetry support in the Azure SDK for .NET](https://devblogs.microsoft.com/azure-sdk/introducing-experimental-opentelemetry-support-in-the-azure-sdk-for-net/)
1. [Instrumentation best practie](https://docs.microsoft.com/en-us/dotnet/core/diagnostics/distributed-tracing-instrumentation-walkthroughs)