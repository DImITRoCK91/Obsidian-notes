
## 1. Назначение
Оператор `switch` — это **оператор множественного выбора**. Он позволяет выбрать один из нескольких блоков кода для выполнения на основе значения выражения. Альтернатива длинной цепочке `if-else if`.

---

## 2. Классический синтаксис (statement)

```java
switch (выражение) {
    case значение1:
        // код
        break;
    case значение2:
        // код
        break;
    default:
        // код по умолчанию
}
```

**Правила:**
- `выражение` должно иметь тип: `byte`, `short`, `char`, `int`, `String` (с Java 7), `enum`, а также соответствующие обёртки (`Byte`, `Short`, `Character`, `Integer`).
- **Нельзя** использовать `long`, `float`, `double`, `boolean`.
- `case` — метки с **константными** значениями (литералы, `final`-переменные, константы `enum`).
- Значения `case` должны быть **уникальны** и совместимы по типу с выражением.
- `default` — необязателен, может стоять в любом месте, но выполняется, если ни один `case` не совпал.

---

## 3. `break` и «проваливание» (fall-through)

- `break` прерывает выполнение `switch` и передаёт управление за его пределы.
- Если `break` отсутствует, управление **«проваливается»** в следующий `case` (fall-through).

```java
int x = 1;
switch (x) {
    case 1:
        System.out.println("one");
    case 2:
        System.out.println("two");
        break;
    default:
        System.out.println("other");
}
// Вывод: one \n two
```

> Fall-through иногда используют намеренно (объединение case), но чаще это источник ошибок.

**Объединение меток:**
```java
switch (day) {
    case 6:
    case 7:
        System.out.println("Выходной");
        break;
    default:
        System.out.println("Рабочий день");
}
```

---

## 4. Работа со `String` (Java 7+)

```java
String s = "Java";
switch (s) {
    case "Java":
        System.out.println("Это Java");
        break;
    case "Python":
        System.out.println("Это Python");
        break;
    default:
        System.out.println("Неизвестно");
}
```
Сравнение происходит через `equals()` (с учётом регистра), при этом `null` вызовет `NullPointerException`.

---

## 5. Работа с `enum`

```java
enum Day { MON, TUE, WED }

Day d = Day.TUE;
switch (d) {
    case MON:
        System.out.println("Понедельник");
        break;
    case TUE:
        System.out.println("Вторник");
        break;
}
```
В `case` для `enum` пишется **имя константы без имени типа** (`TUE`, а не `Day.TUE`).

---

## 6. Важные ограничения

| Ограничение | Пояснение |
|---|---|
| Тип выражения | `byte, short, char, int, String, enum` (+ обёртки) |
| Значения `case` | Должны быть константами времени компиляции |
| Дубликаты | Запрещены |
| `null` | Для `String`/обёрток — `NullPointerException` |
| Область видимости | Переменные, объявленные в `switch`, видны во всех `case` (нужны блоки `{}` при конфликте имён) |

---

## 7. Новая форма `switch` (Java 14+, arrow syntax)

Современная запись без `break` и fall-through:

```java
switch (x) {
    case 1 -> System.out.println("one");
    case 2, 3 -> System.out.println("two or three");
    default -> System.out.println("other");
}
```

**Особенности:**
- Нет «проваливания» — выполняется только один блок.
- Можно использовать `switch` как **выражение** (возвращает значение):

```java
String result = switch (x) {
    case 1 -> "one";
    case 2, 3 -> "few";
    default -> {
        String s = "many";
        yield s;   // возврат значения из блока
    }
};
```

> Это уже не «statement», а «switch expression» — но тесно связано с темой.

---

## 8. Сравнение `switch` и `if-else`

| Критерий | `switch` | `if-else` |
|---|---|---|
| Условие | Равенство конкретным значениям | Любое логическое выражение |
| Типы | Ограниченный набор | Любые |
| Читаемость | Выше при множестве вариантов | Ниже при >3 ветвях |
| Производительность | Обычно быстрее (таблица переходов) | Линейная проверка |

---

## 9. Типичные ошибки

1. **Забытый `break`** → нежелательное fall-through.
2. **Непостоянное значение в `case`** (например, переменная) → ошибка компиляции.
3. **Дубликат `case`** → ошибка компиляции.
4. **`switch` по `long`/`boolean`** → ошибка компиляции.
5. **`switch(null)` для `String`** → `NullPointerException` во время выполнения.
6. **Объявление переменной в одном `case` и использование в другом** → проблемы области видимости (используйте `{}`).

---

## 10. Итоговая шпаргалка

```java
// Классический
switch (expr) { case A: ...; break; default: ...; }

// Стрелочный (Java 14+)
switch (expr) { case A -> ...; default -> ...; }

// Как выражение с yield
var v = switch (expr) { case A -> 1; default -> { yield 0; } };
```

**Ключевые моменты:** ограниченный набор типов, константные `case`, уникальность меток, `break` против fall-through, `default` необязателен, современная форма без проваливания.