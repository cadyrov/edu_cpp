# Объясните разницу между `const` и `constexpr`

## Ответ
`const` и `constexpr` - это два разных механизма в C++, которые имеют различные области применения и поведение.

### const
- Указывает, что значение не может быть изменено после инициализации
- Может быть вычислено как во время компиляции, так и во время выполнения
- Применяется к переменным, методам классов, параметрам функций
- Гарантирует только неизменяемость значения

### constexpr
- Указывает, что значение должно быть вычислено во время компиляции
- Может быть применено к переменным, функциям и конструкторам
- Все вычисления должны быть возможны во время компиляции
- Гарантирует как неизменяемость, так и вычисление во время компиляции

## Примеры кода

### Правильное использование:

```cpp
// const
const int size = 10;
const std::string name = "John";

class MyClass {
    const int value;
public:
    MyClass(int v) : value(v) {}
    int getValue() const { return value; }
};

// constexpr
constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}

constexpr int max_size = 100;
constexpr int computed_value = factorial(5);

// constexpr функции
constexpr int square(int x) {
    return x * x;
}

// constexpr конструкторы
class Point {
    int x, y;
public:
    constexpr Point(int x, int y) : x(x), y(y) {}
    constexpr int getX() const { return x; }
    constexpr int getY() const { return y; }
};
```

### Неправильное использование:

```cpp
// Неправильное использование constexpr
constexpr int getRandomNumber() {
    return rand();  // Ошибка: rand() не может быть вычислен во время компиляции
}

constexpr std::string getMessage() {
    return "Hello";  // Ошибка: std::string не может быть constexpr
}

// Неправильное использование const
const int value;  // Ошибка: const переменная должна быть инициализирована

// Смешивание const и constexpr
const int size = 10;
constexpr int array[size];  // Ошибка: size не является constexpr

// Неправильное использование в классах
class BadClass {
    constexpr int value;  // Ошибка: не-статические члены класса не могут быть constexpr
public:
    constexpr BadClass() {}  // Ошибка: не может инициализировать не-constexpr член
};
```

## Важные замечания
1. `constexpr` подразумевает `const`, но не наоборот
2. `constexpr` функции могут быть вызваны как во время компиляции, так и во время выполнения
3. Все аргументы `constexpr` функции должны быть известны во время компиляции
4. `constexpr` конструкторы могут инициализировать только `constexpr` члены класса
5. `constexpr` переменные должны быть инициализированы константным выражением 