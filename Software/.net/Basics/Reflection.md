---
date: 2023-11-03
---
# Reflection

1. `System.Reflection` фактически предоставляет объектную модель для работы с метаданными модуля.

2. Использование рефлексии сопряжено с проблемами: теряем контроль типов, много полагаемся на строковые имена типов; производительность: на момент компиляции имена типов не известны, поэтому они определяются во время выполнения, производится постоянно поиск строк в метаданных.

3. В связи с озвученными проблемами лучше иметь базовый класс с виртуальными методами или интерфейс. С помощью рефлексии создали экземпляр типа, привели его к базовому типу или интерфейсу, и дальше уже используем в обычном режиме без рефлексии.

4. Отправной точкой является объект Type, представляющий ссылку на тип (обладает небольшой инфой). Его лучше получать с помощью typeof(наиболее быстрый вариант). Более полной инфой обладает тип `TypeInfo`, который можно получить из `Type.GetTypeInfo`.

5. Наиболее подходящий способ создать объект- статический метод `System.Activator`, принимающий ссылку на объект `Type`. В отличии от других методов создания экземпляра, этот способ позволяет создать экземпляр значимого типа без вызова его конструктора. Это актуально, когда у значимого типа нет конструктора. Подходит для всего, кроме массивов и делегатов. Для них есть свои статические методы: `Array.CreateInstance, Delegate.CreateDelegate`.

6. Создание экземпляра обобщённого типа происходит в два этапа: сначала получаем ссылку на открытый тип, потом у неё вызываем MakeGenericType.

7. MemberInfo - базовый класс для  информации о других элементах типа: `FieldInfo`, `MethodInfo`, `PropertyInfo`, `EventInfo`. Сначала у `TypeInfo.GetDeclaredMembers()` получаем коллекцию MemberInfo. Потом перебираем их и пытаемся привести к более конкретному типу, в зависимости от того, что мы ищем. Можно использовать сразу конкретные методы: `GetDeclaredField`, `GetDeclaredMethod` и т.п.

---

## Источники

1. [[CLR via C# book]]

## Ссылки

1. link