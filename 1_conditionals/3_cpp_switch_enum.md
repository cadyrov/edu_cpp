# Оператор switch и перечисления в C++

## Оператор switch
Оператор switch позволяет выполнить разные блоки кода в зависимости от значения выражения.

### Базовый синтаксис
```cpp
switch (выражение) {
    case значение1:
        // код для значение1
        break;
    case значение2:
        // код для значение2
        break;
    default:
        // код для всех остальных случаев
        break;
}
```

### Важные особенности
1. После каждого `case` должен быть `break`
2. Без `break` выполнение продолжится в следующем `case`
3. `default` обрабатывает все необработанные случаи

### Пример использования
```cpp
int n;
std::cin >> n;
switch (n) {
    case 0:
        std::cout << "Ноль" << std::endl;
        break;
    case 1:
        std::cout << "Один" << std::endl;
        break;
    default:
        std::cout << "Другое число" << std::endl;
        break;
}
```

## Перечисления (enum class)

### Создание перечисления
```cpp
enum class Month {
    Jan,
    Feb,
    Mar,
    Apr,
    May,
    Jun,
    Jul,
    Aug,
    Sep,
    Oct,
    Nov,
    Dec
};
```

### Особенности перечислений
1. Значения нумеруются с 0
2. Можно задать явные значения
3. Доступ к значениям через `::` (Month::Jan)
4. Требуется явное приведение типов

### Пример с явными значениями
```cpp
enum class Day {
    Mon = 1,
    Tue,    // будет 2
    Wed,    // будет 3
};
```

### Использование в switch
```cpp
enum class Weekday { Mon = 1, Tue, Wed, Thu, Fri, Sat, Sun };

Weekday day = Weekday::Mon;
switch (day) {
    case Weekday::Mon:
    case Weekday::Tue:
    case Weekday::Wed:
    case Weekday::Thu:
    case Weekday::Fri:
        std::cout << "Будний день" << std::endl;
        break;
    case Weekday::Sat:
    case Weekday::Sun:
        std::cout << "Выходной день" << std::endl;
        break;
}
```

## Разрешённые типы для switch
- Целочисленные типы (int, char, size_t)
- Перечисления (enum class)
- НЕ разрешены: строки, числа с плавающей точкой, bool

## Приведение типов
```cpp
// Из числа в перечисление
int n = 4;
Month m = static_cast<Month>(n);

// Из перечисления в число
int k = static_cast<int>(Month::Jan);  // k = 0
``` 