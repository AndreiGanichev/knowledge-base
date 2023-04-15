---
date: 2023-03-23
tags:
    - tag
---

**Хэш-функция** представляет данные произвольной длины в виде данных фиксированной длины.

**Коллизия** - явление, когда разные исходные строки после пропускания через хэш-функцию имеют одинаковый хэш.

### Варианты
1. криптографические ([SHA-2](https://en.wikipedia.org/wiki/SHA-2))
1. некриптографические ([MD5](https://en.wikipedia.org/wiki/MD5))

### Требования к хэш-функции
1. *быстрая*. Обратная сторона - чем быстрее функция, тем легче ее взламывать перебором [[Brute force]].
1. (надежная)(не падает с ошибкой)
1. лавинный эффект - минимальное изменение исходной строки приводит к кардинальномму изменению хэша
1. невозможность восстановления исходного сообщения по хэшу
1. минимальная вероятность коллизии - важно в первуюочередь для криптографических функций

#### Hash partitioning. Когда коллизия - это хорошо
Можем взять уникальный ID, взять его хэш и партиционировать именно по хэшу. Это может помочь, когда необходимо ограничить или строго задать кол-во партиций.

### Сферы применения
1. для вычисления ключа в [[Hash table]]
1. hash partitioning
1. для пользователя хранится не сам пароль, а его хэш. Даже если злоумышленник украдет БД, он неполучит доступ к самому паролю.
1. для важных документов вычисляем хэш. Подлинность документа можно проверить проверив, совпадает ли хэш с исходным
1. в электронном ДО подписываются не сами документы(могут быть большими по объему), а их хэши

### Salt

Хэши самых популярных паролей известны(см. [Rainbow table](https://en.wikipedia.org/wiki/Rainbow_table)). А значит имея на руках только украденные хэши паролей, злоумышленник может легко определить такие пароли. Чтобы избежать этого, к самому паролю перед хэшированием добавляется "соль" - произвольная строка.

---

### Источники:
1. link

### Ссылки:
1. link