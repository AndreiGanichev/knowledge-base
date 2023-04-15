---
date: 2023-04-09
tags:
    - tag
---

# Виды

## Backward compatibility

> Newer code can read data that was written by older code. [[Designing Data-Intensive Applications book]]. Chapter 4

> Compiler *backward compatibility* may refer to the ability of a compiler of a newer version of the language to accept programs or data that worked under the previous version. [Wikipedia](https://en.wikipedia.org/wiki/Backward_compatibility#In_software)

## Forward compatibility

> Older code can read data that was written by newer code.[[Designing Data-Intensive Applications book]]. Chapter 4

# Форматы

Требование совместимости в разной степени поддерживается для разных форматов сериализации.

### Бинарная

Зависит от конкретного формата и возможностей схемы. Подробнее можно почитать в [[Designing Data-Intensive Applications book]]. Chapter 4. Formats for encoding data.

### Текстовая

Добавление новых опциональных полей в документ(например JSON) сохраняет прямую и обратную совместимость. Надо только обеспечить, что десериализатор не теряет неизвестные ему поля. Старый код, который не знает о добавленных в документ полях, должен прихранить эти поля при десериализации, чтобы потом добавить их обратно при сериалиции и сохранении.


---

### Источники:
1. link

### Ссылки:
1. [[Serialization]]