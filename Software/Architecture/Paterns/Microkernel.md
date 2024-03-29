---
date: 2023-01-21
tags:
    - tag
---
# Microkernel

Архитектура предполагает компоненты:

1. ядро(kernel) содержит базовый абстрактный функционал, который редко меняется. Ядро не зависит от плагинов.
1. плагины(plugin), конкретные реализации, которые обеспечивают вариативность поведения системы.

Примеры приложений с такой архитектурой: IDE, браузеры

В том смысле, что ядро закрыто для изменения, но открыто к расширению через плагины, делает идею этой архитектуры схожей с [[Open close principle]].

Главная и самая сложная задача - **сделать плагины независимы друг от друга**. Это уменьшает [[Architecture quantum]] до размера плагина и делает возможным их независимое развитие.

---

## Источники

1. [[Building evolutionary architectures book]]

## Ссылки

1. link
