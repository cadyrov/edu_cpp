# Что такое `make_shared`?

## Ответ
`make_shared` - это функция-шаблон, которая создает `shared_ptr` безопасным и эффективным способом. В отличие от прямого создания `shared_ptr`, `make_shared` выполняет одно выделение памяти для объекта и блока управления (control block), что делает его более эффективным. Эта функция была добавлена в C++11 и является предпочтительным способом создания `shared_ptr`.

## Примеры кода

### Правильное использование:
```cpp
// Создание shared_ptr для одиночного объекта
auto ptr1 = std::make_shared<int>(42);

// Создание shared_ptr для пользовательского класса
class MyClass {
public:
    MyClass(int x, double y) : x_(x), y_(y) {}
private:
    int x_;
    double y_;
};

auto obj = std::make_shared<MyClass>(42, 3.14);

// Безопасное создание в функции
void safeFunction() {
    auto ptr = std::make_shared<int>(42);
    // Если здесь произойдет исключение, память будет корректно освобождена
    throw std::runtime_error("Error");
}

// Создание shared_ptr из weak_ptr
std::weak_ptr<int> weak = ptr1;
if (auto shared = weak.lock()) {
    // Используем shared
}
```

### Неправильное использование:
```cpp
// Ошибка: прямое использование new
std::shared_ptr<int> ptr(new int(42)); // Менее эффективно и небезопасно при исключениях

// Ошибка: создание shared_ptr из raw pointer
int* raw = new int(42);
std::shared_ptr<int> ptr1(raw);
std::shared_ptr<int> ptr2(raw); // Ошибка: двойное удаление

// Ошибка: неправильное использование make_shared для массива
auto arr = std::make_shared<int[]>(10); // Ошибка: make_shared не поддерживает массивы
```

## Комментарии
- `make_shared` более эффективен, чем прямое создание `shared_ptr`
- `make_shared` обеспечивает безопасность при исключениях
- `make_shared` не поддерживает массивы (используйте `make_unique` для массивов)
- Память освобождается только когда уничтожается последний `shared_ptr` и `weak_ptr`
- Не создавайте `shared_ptr` из raw pointer, если этот указатель уже управляется другим `shared_ptr` 