# Что такое `std::variant`?

## Ответ
`std::variant` - это шаблонный класс, введенный в C++17, который представляет типобезопасный union. Он может хранить значение одного из нескольких альтернативных типов, при этом тип значения может быть определен во время выполнения.

### Основные характеристики:

1. **Типобезопасность**
   - Гарантирует, что значение всегда имеет один из указанных типов
   - Не может быть пустым (в отличие от std::optional)
   - Всегда содержит значение одного из альтернативных типов

2. **Доступ к значению**
   - std::get<T> для прямого доступа
   - std::get_if<T> для безопасного доступа
   - std::visit для обработки всех возможных типов

3. **Проверка типа**
   - index() для получения индекса текущего типа
   - holds_alternative<T> для проверки типа
   - valueless_by_exception() для проверки на исключение

## Примеры кода

### Правильное использование:

```cpp
// Базовое использование
std::variant<int, std::string, double> v;
v = 42;  // Хранит int
v = "hello";  // Хранит string
v = 3.14;  // Хранит double

// Безопасный доступ к значению
if (auto pval = std::get_if<int>(&v)) {
    std::cout << "Integer: " << *pval << '\n';
}

// Использование visit
struct Visitor {
    void operator()(int i) { std::cout << "int: " << i << '\n'; }
    void operator()(const std::string& s) { std::cout << "string: " << s << '\n'; }
    void operator()(double d) { std::cout << "double: " << d << '\n'; }
};

std::visit(Visitor{}, v);

// Работа с пользовательскими типами
struct Point { int x, y; };
struct Circle { int radius; };
struct Rectangle { int width, height; };

using Shape = std::variant<Point, Circle, Rectangle>;

void draw(const Shape& shape) {
    std::visit([](const auto& s) {
        using T = std::decay_t<decltype(s)>;
        if constexpr (std::is_same_v<T, Point>) {
            std::cout << "Drawing point at (" << s.x << "," << s.y << ")\n";
        } else if constexpr (std::is_same_v<T, Circle>) {
            std::cout << "Drawing circle with radius " << s.radius << "\n";
        } else if constexpr (std::is_same_v<T, Rectangle>) {
            std::cout << "Drawing rectangle " << s.width << "x" << s.height << "\n";
        }
    }, shape);
}
```

### Неправильное использование:

```cpp
// Неправильный доступ к значению
std::variant<int, std::string> v = 42;
std::string s = std::get<std::string>(v);  // Выбросит std::bad_variant_access

// Неправильное использование с неразрешенными типами
std::variant<int, std::string> v;
v = 3.14;  // Ошибка компиляции: double не является альтернативным типом

// Неправильное использование с указателями
std::variant<int*, std::string*> v;  // Опасность: может привести к утечке памяти
// Лучше использовать:
std::variant<std::unique_ptr<int>, std::unique_ptr<std::string>> v;

// Неправильное использование в циклах
std::vector<std::variant<int, std::string>> values = {1, "hello", 2};
for (const auto& v : values) {
    std::cout << std::get<int>(v) << '\n';  // Небезопасно: нет проверки типа
}
// Лучше использовать:
for (const auto& v : values) {
    std::visit([](const auto& val) {
        std::cout << val << '\n';
    }, v);
}

// Неправильное использование с временными объектами
std::variant<std::string> v = std::string("temp");  // Создает лишнюю копию
// Лучше использовать:
std::variant<std::string> v(std::in_place_type<std::string>, "temp");
```

## Важные замечания
1. Всегда проверяйте тип значения перед доступом
2. Используйте std::visit для обработки всех возможных типов
3. Избегайте использования variant с сырыми указателями
4. При работе с variant в циклах используйте std::visit
5. Для пользовательских типов предпочтительно использовать in-place конструирование 