---
date: 2023-11-03
---
# Custom attributes

1. Custom attributes декларативно добавляют информацию к различным элементам кода. Эта информация хранится в метаданных модуля. При компиляции компилятор создаёт экземпляр атрибута, сериализует его в ``byte[]`` и сохраняет в метаданные. При выполнении кода производится чтение из метаданных, десериализация и использование экземпляра атрибута.

2. Атрибут **можно применить**: сборки, модули, типы(класс, структура, перечисление, интерфейс, делегат), поля, методы(в т.ч. конструкторы), параметры и возвращаемые значения методов, свойства, события, параметры обобщённого типа.

3. При назначении атрибутов можно использовать **префиксы**, указывающие к чему именно нужно применить: assembly, module, field, method и др. В некоторых случаях компилятор не может сам определить к чему относится атрибут и префиксы обязательны: сборка, модуль. Плюс например, когда хотим навесить атрибут на поля и методы, создаваемым компилятором для события. Тогда прям на событие вешаем атрибуты с префиксом field, method.

4. При назначении атрибута нужно передавать параметры, имеющиеся в конструкторе, плюс можно синтаксисом именованных параметров назначить значения открытых свойств и полей атрибута.

5. Атрибут стоит рассматривать как логический контейнер состояния. В классе-атрибуте можно определять только поля и свойства. При этом для них можно использовать только примитивные типы(без dynamic), Type и перечисления, плюс массивы этих типов с нулевой нижней границей. Свойство типа ```Type``` в конструктор нужно передавать используя typeof().

6. С помощью свойства ```Inherited```(по умолчанию ```true```) атрибута ```AttributeUsage``` можно определить будет ли атрибут, применённый к элементу базового класса, также применяться к этому элементу в классе-наследнике. Наследуется атрибуты только для: классов, полей и все что связано с методами(свойств, событий, параметров и возвращаемых значений).

7. Для анализа атрибутов в коде используется рефлексия: методы ```IsDefined, GetCustomAttribute, GetCustomAttributes. IsDefined``` есть прямо для типа ```Type```, а для остального есть перечисленные методы расширения. Эти методы ищут указанные атрибут или производные от него. ```IsDefined``` быстрый, потому что не десериализует инфу из метаданных, а просто проверяет наличие.

8. Рассмотренные выше методы, кроме ```IsDefined```, каждый раз приводят к созданию экземпляра атрибута: вызов конструктора типа, конструктора экземпляра, методов set свойств. Если есть подозрение, что в них может быть вредоносный код, то можно избежать этих вызовов и использовать ```System.Reflection.CustomArtributeData```.

9. Проверку наличия атрибутов  не только в конкретном классе, но и в иерархии наследования поддерживают только методы для ```Type, MethodInfo и Attribute```. Так что для типа и метода можно использовать их "родные" методы, а для всего остального - методами для ```Attribute```.

10. Для **сравнения** двух экземпляров атрибуты можно использовать методы класса ```Artribute```: ```Equals```(использует рефлексию), ```Match```(просто вызывает ```Equals```). Но их можно переопределить.

---

## Источники

1. [[CLR via C# book]]

## Ссылки

1. link