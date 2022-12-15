---
date: 2022-08-31
tags:
    - boundaries
---

Если *Subdomains* - это инструмент, который позволяет разделить предметную область на поддомены, определить их тип и соответственно сформировать стратегию их реализации в целом. Дальше нужно углубиться в предметную область и перенять *ментальную модель* экспертов.

> Наша цель создать ПО, которое бы создали эксперты предметной области, если бы уменли программировать. Вон Вернон

### Сломанный телефон

Иногда процессы разработки строятся так, что "доступ" к экспертам имеют только отдельные роли, например аналитики или PO, а разработчики уже получают на вход конкретные требования к системе, которые необходимо реализовать. Потом на основании этих требований производится проектирование системы. На основе олученного дизайна пишется исходный код. В итоге имеем последовательность: *домен -> требования -> дизайн -> исходный код*. У такого подхода есть недостатки:
1. разработчики не работают с реальными бизнес проблемами, а работают уже с требованиями к решению
1. каждое звено между доменными экспертами и исходным кодом - это перевод(интерпретация) знаний экспертов, а значит может вносить искажения
1. в случае обнаружения вопроса/бага большая цепочка для того, чтобы узнать подробности у экспертов

###  Как проблему предлагает решать DDD

DDD предлагает, во-первых, разработчикам коммуницировать напрямую с экспертами, получая доступ к оригинальным знаниям, а не переводу. Идея максимально простая и даже очевидная: если разработчикам нужно перенять ментальную модель домена от экспертов, то давайте разработчики будут общаться с экспертами. Для этого DDD предоставляет инструмент - *Ubiquotous language*.

UL - это язык предметной области, т.е. экспертов, а не технарей(никаких синглтонов, абстрактных фабрик, таблиц БД). Это с одной стороны дает более полное погружение в домен, с другой - раскрепощает экспертов, потому что обсуждение проходит на понятном им языке.

Разработчики не должны становиться доменными экспертами, но *UL* позволяет максимально упростить коммуникации с экспертами, чтобы всегда можно было быстро и точно получить нужную информацию.

UL, а не его интерпретации, используетя на всех уровнях(от формулирования задач до проектирования и исходного кода), поэтому на всех уровнях можно обсуждать и уточнять решения с экспертами. Так UL не только будет использоваться как источник правды для нейминга при проектировании и в исходном коде, но и влиять на принимаемые разработчиками решения.

> Our goal is to use UL to drive software design decisions. Learning DDD. Chapter 3. Managing domain complexity. Vladik Khononov

UL - это не только и не столько глоссарий. Активная постоянная коммуникация с экспертами и на языке экспертов позволяет выявлять тонкости домена, позволяет находить пробелы в знаниях в том числе у самих экспертов.

Критически важная особенность UL - он должен исопльзоваться постоянно: коммуникации, проектирование, документация, тесты, код. А в ходе постоянного использования он постоянно эволюционирует, потому что обязательно будут находится неточности и новые знания. Уточнять *UL* может любой член команды и эксперты.

Основное качество, которое нужно поддерживать в *UL* - **непротиворечивость**. Не должно быть:
1. терминов, обозначающих разные сущности домена
1. нескольких терминов-синонимов, описывающие одну сущность

### Модель

*UL* - это фактически **модель предметной области**, он содержит сущности, описывает их поведение, инварианты и взаимосвязи.

### Как фиксировать UL

1. глоссарий
2. Геркин тесты


---

### Источники:
1. 1. Learning DDD. Chapter 3. Discovering domain knowledge. Vladik Khononov

### Ссылки:
1. #[[Strategic patterns]]
1. [[Subdomain]]