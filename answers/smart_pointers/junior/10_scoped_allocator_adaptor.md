# Как работает `scoped_allocator_adaptor`?

## Ответ
`scoped_allocator_adaptor` - это шаблонный класс, который позволяет передавать аллокатор вложенным объектам в контейнерах. Он особенно полезен при работе с контейнерами, содержащими другие контейнеры, где нужно обеспечить использование одного и того же аллокатора на всех уровнях вложенности.

## Примеры кода

### Правильное использование:
```cpp
// Создание scoped_allocator_adaptor
using MyAllocator = std::allocator<int>;
using ScopedAlloc = std::scoped_allocator_adaptor<MyAllocator>;

// Использование в контейнере
std::vector<std::vector<int, ScopedAlloc>, ScopedAlloc> nested_vec;

// Создание вложенных контейнеров
nested_vec.emplace_back(); // Внутренний вектор получит тот же аллокатор
nested_vec[0].push_back(42);

// Использование с пользовательским аллокатором
template<typename T>
class CustomAllocator {
public:
    using value_type = T;
    T* allocate(std::size_t n) {
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }
    void deallocate(T* p, std::size_t n) {
        ::operator delete(p);
    }
};

using CustomScopedAlloc = std::scoped_allocator_adaptor<CustomAllocator<int>>;
std::vector<std::vector<int, CustomScopedAlloc>, CustomScopedAlloc> custom_nested_vec;
```

### Неправильное использование:
```cpp
// Ошибка: неправильное использование scoped_allocator_adaptor
std::scoped_allocator_adaptor<std::allocator<int>> alloc;
int* ptr = alloc.allocate(1); // Ошибка: scoped_allocator_adaptor не предназначен для прямого выделения памяти

// Ошибка: смешивание разных типов аллокаторов
std::vector<std::vector<int, std::allocator<int>>, ScopedAlloc> mixed_vec; // Неправильное использование

// Ошибка: неправильная вложенность
std::vector<int, ScopedAlloc> single_vec; // Бессмысленное использование scoped_allocator_adaptor
```

## Комментарии
- `scoped_allocator_adaptor` предназначен для работы с вложенными контейнерами
- Обеспечивает передачу аллокатора на все уровни вложенности
- Полезен при работе с контейнерами, содержащими другие контейнеры
- Не предназначен для прямого выделения памяти
- Упрощает управление памятью в сложных структурах данных 