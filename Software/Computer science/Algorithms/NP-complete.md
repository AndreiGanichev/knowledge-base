---
date: 2023-11-12
---
# NP-complete

Упрощенно, это задачи не имеющие быстрого(полиномиального) решения.

## Комивояжера

Заключающаяся в поиске самого выгодного маршрута, проходящего через указанные города хотя бы по одному разу с последующим возвратом в исходный город.

Количество всех возможных маршрутов для n городов вычисляется как количество *перестановок без повторений*. Полный перебор имеет [[Спринт 1|временную сложность]] *O(n!)*.

## Покрытие множества

Наивный алгоритм предполагает перебор всех возможных сочетаний радиостанций и поиск минимально необходимого. Поиск всех возможных сочетаний заключается в поиске всех возможных подмножеств исходного множества станций([степенное множество](https://en.wikipedia.org/wiki/Power_set)): для n городов вычисляется как количество *размещений с повторениями*: составляем таблицу в которой в столбцах записаны станции, а в строках все варианты их сочетания(водмножества). В каждой ячейке отмечаем 1, если станци включена в данный набор и 1 - если включена.  Полный перебор имеет [[Спринт 1|временную сложность]] *O(n^2)*.

### Пример

1. Представим себе, что для выполнения какого-то задания необходим некий набор навыков S. Также есть группа людей, каждый из которых владеет некоторыми из этих навыков. Необходимо сформировать наименьшую подгруппу, достаточную для выполнения задания, т. е. включающую в себя носителей всех необходимых навыков.

1. Есть набор радиостанций, каждая из которых работает в определенных штатах. Необходимо определить минимальный набор станций, которые обеспечат покрытие всех штатов страны.

## Признаки NP-полных задач

С помощью [[Спритн 6. Графы. Алгоритмы|BFS]] можно определить кратчайший путь между вершинами графа. Но вот, если требуется найти кратчайший путь, соединяющий несколько точек, то это уже задача о комивояжере.

---

## Источники

1. [[Грокаем алгоритмы]]

## Ссылки

1. [Travelling salesman problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem)
1. [Set cover problem](https://en.wikipedia.org/wiki/Set_cover_problem)
1. [[Combinatorics]]