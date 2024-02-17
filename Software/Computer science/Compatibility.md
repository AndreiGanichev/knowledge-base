---
date: 2023-04-09
tags:
    - tag
---
# Compatibility

> Compatibility is a relationship between one process that encodes the data, and another process that decodes it. [[Designing Data-Intensive Applications book]]. Chapter 4. Models of Dataflow.

Совместимость важна для разных видов приложений:

1. **серверные** приложения могут поддерживать *rolling upgrade* и становится нормальным, что одновременно работают разные версии приложения.
1. обновление **клиентских** приложений часто находится вне зоне влияния разработчиков(например мобильные приложения), а значит  с одним и тем же сервером работает множество версий клиента.

Совместимость оказывает большое влияние на [[Evolvability]]. В случае, если обеспечивается прямая и обратная совместимость мы можем развивать и деплоить сервисы независимо, можем позволить одному сервису быть запущенным с разными версиями одновременно.

## Виды

### Backward compatibility

> Newer code can read data that was written by older code. [[Designing Data-Intensive Applications book]]. Chapter 4

> Compiler *backward compatibility* may refer to the ability of a compiler of a newer version of the language to accept programs or data that worked under the previous version. [Wikipedia](https://en.wikipedia.org/wiki/Backward_compatibility#In_software)

### Forward compatibility

> Older code can read data that was written by newer code.[[Designing Data-Intensive Applications book]]. Chapter 4

## Форматы сериализации и совместимость

Требование совместимости в разной степени поддерживается для разных форматов сериализации.

### Бинарная

Зависит от конкретного формата и возможностей схемы. Подробнее можно почитать в [[Designing Data-Intensive Applications book]]. Chapter 4. Formats for encoding data.

### Текстовая

Добавление новых опциональных полей в документ(например JSON) сохраняет прямую и обратную совместимость. Надо только обеспечить, что десериализатор не теряет неизвестные ему поля. Старый код, который не знает о добавленных в документ полях, должен прихранить эти поля при десериализации, чтобы потом добавить их обратно при сериалиции и сохранении. [Реализация в .NET](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/handle-overflow?pivots=dotnet-7-0)

## Важность совместимости в индустрии

### Windows API

Джоэль Спольски пишет, что главным конкурентным преимуществом Windows было сохранение совместимость API по входу развития ОС [1]. На это было направлена значительная часть ресурсов. Но в какой-то момент стратегия изменилась и ради скорости начали жертвовать совместимостью. Что по мнению Спольски было ошибкой.

### Golang

Автор языка описывает две причины из-за чего язык стал популярным: технологическую (first class поддержка параллелизма) и политическую.

> The political success was the firm promise made about compatibility for Go version 1. That stability — Go programs written in 2012 will still compile and run perfectly today — was a huge enabler for growth. Companies could use the language with confidence that we would not break their software.

---

## Источники

1. [[Джоэл о программировании]]. Как Microsoft проиграла войну API.
1. [Rob Pike interview: “Go has indeed become the language of cloud infrastructure“](https://evrone.com/rob-pike-interview)

## Ссылки

1. [[Serialization]]

