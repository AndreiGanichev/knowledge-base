---
date: 2023-04-27
---
# Database schema

Несмотря на то, что в [[Document data model]] отсутствует схема в классическом(как реляционных СУБД) понимании и часто эту модель называют *schemaless*, на самом деле правильнее к этому вопросу подходить с точки зрения наличия схемы на запись (*schema-on-write*) и схемы на чтение (*schema-on-read*).

В случае [[Relational model]] обычно существует схема на запись и в случае если мы пытаемся записать что-то не то - СУБД не даст нам этого сделать. В случае документной модели мы пишем все что угодно, но чтобы пользоваться этими данными нам надо преобразовать вычитанные данные в структуру, которая известна приложению. Соответственно данные в БД должны соответствовать структуре данных приложения, которую можно рассматривать как схема на чтение.

> Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-time) type checking.

## Evolvability

> The database can evolve right alongside the architecture as long as developers apply proper engineering practices such as continuous integration, source control, and so on.

> Developers must treat changes to database structure the same way they treat source code: tested, versioned, and incremental.
[[Building evolutionary architectures book]]. Chapter 5.

Одним из способов достижения выше описанных целей является использование [[Database migrations]]#.

## Constraints

Всегда нужно выбирать наиболее строгие ограничения, какие только возможны. Этот совет касается разных аспектов:

1. ограничения на тип столбца
1. ограничение на nullability столбца
1. ограничение на уникальность данных

Это необходимо для сохранения [[Compatibility]]: ослабить ограничения в дальнейшем проще, чем усилить(могут уже быть данные нарушающие более сильное ограничение)

---

## Источники

1. [[Designing Data-Intensive Applications book]]. Chapter 4.

## Ссылки

1. [[Evolutionary data]]
