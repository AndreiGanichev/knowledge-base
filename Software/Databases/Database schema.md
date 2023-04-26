---
date: 2023-04-27
---
# Database schema

Несмотря на то, что в [[Document data model]] отсутствует схема в классическом(как реляционных СУБД) понимании и часто эту модель называют *schemaless*, на самом деле правильнее к этому вопросу подходить с точки зрения наличия схемы на запись (*schema-on-write*) и схемы на чтение (*schema-on-read*).

В случае [[Relational model]] обычно существует схема на запись и в случае если мы пытаемся записать что-то не то - СУБД не даст нам этого сделать. В случае документной модели мы пишем все что угодно, но чтобы пользоваться этими данными нам надо преобразовать вычитанные данные в структуру, которая известна приложению. Соответственно данные в БД должны соответствовать структуре данных приложения, которую можно рассматривать как схема на чтение.

> Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-time) type checking.

---

## Источники

1. [[Designing Data-Intensive Applications book]]. Chapter 4.

## Ссылки

1. [[Evolutionary data]]