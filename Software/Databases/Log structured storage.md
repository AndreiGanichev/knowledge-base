---
date: 2023-03-27
tags:
    - tag
---
# Log structured storage

## Достоинства

1. запись очень быстрая - производится просто в конец файла

## Недостатки

## Область применения

> Log-structured storage is used everywhere: from the flash translation layer, to filesystems and database systems. [[Database Internals book]].Chapter 7

[[LSM tree]] берет свое начало с работы “The Design and Implementation of a Log-Structured File System”, которая как видно из названия посвящена файловым системам.

---

## Источники

1. [[Database Internals book]]
1. [[Designing Data-Intensive Applications book]]. Chapter 3.

## Ссылки

1. [[LSM tree]]#
1. Mendel Rosenblum and John K. Ousterhout: “The Design and Implementation of a Log-Structured File System” ACM Transactions on Computer Systems, volume 10, number 1, pages 26–52, February 1992.