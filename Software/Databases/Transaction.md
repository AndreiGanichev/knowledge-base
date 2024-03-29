---
date: 2023-03-20
tags:
    - history
---

# Transaction

## Откуда пришел термин

> In the early days of business data processing, a write to the database typically corresponded to a commercial transaction taking place: making a sale, placing an order with a supplier, paying an employee’s salary, etc. As databases expanded into areas that didn’t involve money changing hands, the term transaction nevertheless stuck, referring to a **group of reads and writes that form a logical unit**.

## ACID

> A transaction needn’t necessarily have ACID properties. Transaction processing just means allowing clients to make low-latency reads and writes— as **opposed to batch processing jobs**, which only run periodically (for example, once per day).

## Transactional boundaries

> While systems often cannot avoid transactions, architects should try to **limit transactional contexts as much as possible** because they form a tight [[Coupling|coupling knot]], hampering the ability to change components or services without affecting others. More importantly, architects should take aspects like transactional boundaries into account when thinking about architectural changes(см. [[Evolutionary architecture]]).

---

## Источники

1. [[Designing Data-Intensive Applications book]] Chapter 3.

## Ссылки

1. [[ACID]]
1. [[OLTP]]
1. [[Transaction manager]]
