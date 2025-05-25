# Как использовать `make_unique`?

## Ответ
`make_unique` - это функция-шаблон, которая создает `unique_ptr` безопасным способом. Она была добавлена в C++14 и является предпочтительным способом создания `unique_ptr`. `make_unique` обеспечивает безопасность при исключениях и делает код более чистым и понятным.

## Примеры кода

### Правильное использование:
```cpp
// Создание unique_ptr для одиночного объекта
auto ptr1 = std::make_unique<int>(42);

// Создание unique_ptr для массива
auto arr = std::make_unique<int[]>(10);

// Создание unique_ptr для пользовательского класса
class MyClass {
public:
    MyClass(int x, double y) : x_(x), y_(y) {}
private:
    int x_;
    double y_;
};

auto obj = std::make_unique<MyClass>(42, 3.14);

// Безопасное создание в функции
void safeFunction() {
    auto ptr = std::make_unique<int>(42);
    // Если здесь произойдет исключение, память будет корректно освобождена
    throw std::runtime_error("Error");
}
```

### Неправильное использование:
```cpp
// Ошибка: прямое использование new
std::unique_ptr<int> ptr(new int(42)); // Небезопасно при исключениях

// Ошибка: неправильное создание массива
auto arr = std::make_unique<int>(10); // Создает один int со значением 10, а не массив

// Ошибка: утечка памяти при исключениях
void unsafeFunction() {
    int* raw = new int(42);
    std::unique_ptr<int> ptr(raw);
    throw std::runtime_error("Error"); // Потенциальная утечка памяти
}
```

## Комментарии
- Всегда предпочитайте `make_unique` прямому использованию `new`
- `make_unique` обеспечивает безопасность при исключениях
- Для массивов используйте `make_unique<T[]>(size)`
- `make_unique` делает код более читаемым и поддерживаемым
- Не смешивайте `make_unique` с raw pointers 