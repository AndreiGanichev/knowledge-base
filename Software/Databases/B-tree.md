---
date: 2023-03-08
tags:
    - tag
---
# B-tree

**Balanced tree** - наиболее широко распространенная в СУБД структура данных, давно известна и хорошо изучена. Представляет собой сбалансированное(глубина log(n)) дерево. В вершинах дерева расположены страницы фиксированного размера, обычно 4 Кб. Такой подход перекликается с тем как работают диски - тоже читают и пишут блоками. Является изменяемой(*mutable*) структурой. С этой точки зрения B-tree противопоставляется [[Log structured storage]].
Несмотря на противопоставление их успешно используют и вместе:

- индексы для отдельных сегментов(файлов) [[LSM tree]] могут выполняться в виде B-tree. 
- *write ahead log* используется для того, чтобы сделать B-tree надежной и не терять изменения.

По умолчанию(всегда надо смотреть на конкретный профиль нагрузки) **оптимизирована для чтения**.

B-tree может быть достаточно эффективна реализована на диске([[Data structures]]), потому что при большом кол-ве ключей на странице:

1. поиск требует небольшого кол-ва переходов (*seek*) между страницами
1. заполнение страницы происходит не сразу, а значит требуется не так много операций перестроений(split, merge) и ребалансировки
Меньше размер ключа -> больше ключей помещается на одной странице -> выше *branching factor* (*fanout*) и меньше глубина дерева.

## Достоинства

1. отсутствует [[Space amplification]] - данные хранятся в одном экземпляре
1. структура данных давно известна, хорошо изучена и оптимизирована

## Недостатки

1. [[Random write]] - сначала ищется нужная страница и только потом она точечно перезаписывается
1. [[Write amplification]] - даже если меняется только часть страницы, она все равно перезаписывается на диск целиком. Если при изменении еще приходится делать *split*, то перезаписать придется три страницы
1. страницы имеют фиксированную длину и некоторую область приходится резервировать на будущее, чтобы удовлетворить последующие вставки ключей -> *фрагментация* диска
1. чтобы не "сломать" структуру дерева при конкуррентной записи приходится использовать *lock*. Альтернативой является [[Latch]] - более легковесный механизм.

## Split and merge

## Оптимизации

## Монотонно возрастающий ключ

[[Primary key type]]

Для эффективной вставки в B-tree лучше, чтобы ключ монотонно возрастал. В этом случае вставка будет осуществляться в крайний правый лист. Это дает:

1. снижение накладных расходов на балансировку дерева, уменьшение кол-ва *splits, merges*. Нода просто заполняется по порядку, после заполнения запись начинается в следующую ноду
1. самые последние ключи локализованы на самых правых листьях. Значит, если наиболее интенсивное чтение приходится на самые свежие вставки, то эти ноды можно держать в кэше и оптимизирвоать чтение.

> Many database systems use auto-incremented monotonically increasing values as primary index keys. This case opens up an opportunity for an optimization, since all the insertions are happening toward the end of the index (in the rightmost leaf), so most of the splits occur on the rightmost node on each level. Moreover, since the keys are monotonically incremented, given that the ratio of appends versus updates and deletes is low, nonleaf pages are also less fragmented than in the case of randomly ordered keys.

PostgreSQL is calling this case a *fastpath*. When the inserted key is strictly greater than the first key in the rightmost page, and the rightmost page has enough space to hold the newly inserted entry, the new entry is inserted into the appropriate location in the *cached* rightmost leaf, and the whole read path can be skipped.

SQLite has a similar concept and calls it *quickbalance*. When the entry is inserted on the far right end and the target node is full (i.e., it becomes the largest entry in the tree upon insertion), instead of rebalancing or splitting the node, it allocates the new rightmost node and adds its pointer to the parent. Even though this leaves the newly created page nearly empty (instead of half empty in the case of a node split), it is very likely that the node will get filled up shortly. [[Database Internals book]]. Chapter 4.

---

## Источники

1. link

## Ссылки

1. [[Index]]
1. [[Спринт 5. Сбалансированные дервеья]]
