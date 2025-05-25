# Что такое `stack` и `queue`?

## Ответ
`std::stack` и `std::queue` - это адаптеры контейнеров, которые предоставляют ограниченный интерфейс для работы с данными.

`std::stack`:
- Реализует структуру данных "стек" (LIFO - Last In First Out)
- По умолчанию использует `std::deque` как базовый контейнер
- Основные операции: push, pop, top

`std::queue`:
- Реализует структуру данных "очередь" (FIFO - First In First Out)
- По умолчанию использует `std::deque` как базовый контейнер
- Основные операции: push, pop, front, back

## Примеры кода

### Правильное использование

```cpp
// Работа со стеком
std::stack<int> s;

// Добавление элементов
s.push(1);
s.push(2);
s.push(3);

// Получение верхнего элемента
int top = s.top();  // 3

// Удаление верхнего элемента
s.pop();  // Удаляет 3

// Проверка размера и пустоты
size_t size = s.size();
bool empty = s.empty();

// Работа с очередью
std::queue<int> q;

// Добавление элементов
q.push(1);
q.push(2);
q.push(3);

// Получение первого и последнего элементов
int front = q.front();  // 1
int back = q.back();    // 3

// Удаление первого элемента
q.pop();  // Удаляет 1

// Проверка размера и пустоты
size = q.size();
empty = q.empty();
```

### Неправильное использование

```cpp
// Неправильно: попытка прямого доступа к элементам
std::stack<int> s = {1, 2, 3};  // Ошибка! Нет конструктора от initializer_list
s[0];  // Ошибка! Нет оператора []

// Правильно: последовательное добавление элементов
std::stack<int> s;
s.push(1);
s.push(2);
s.push(3);

// Неправильно: использование stack для частого доступа к элементам
std::stack<int> s;
for (int i = 0; i < 1000; ++i) {
    s.push(i);
}
for (int i = 0; i < 1000; ++i) {
    int value = s.top();  // Неэффективно для частого доступа
    s.pop();
}

// Правильно: использование vector для частого доступа
std::vector<int> v;
for (int i = 0; i < 1000; ++i) {
    v.push_back(i);
}
for (int i = 0; i < 1000; ++i) {
    int value = v[i];  // Эффективно для частого доступа
}
```

## Рекомендации по использованию

1. Используйте `std::stack` когда:
   - Нужна структура данных LIFO
   - Требуется только доступ к верхнему элементу
   - Нужна простота использования
   - Важна эффективность операций push/pop

2. Используйте `std::queue` когда:
   - Нужна структура данных FIFO
   - Требуется доступ к первому и последнему элементам
   - Нужна простота использования
   - Важна эффективность операций push/pop

3. Не используйте `stack`/`queue` когда:
   - Нужен произвольный доступ к элементам
   - Требуется доступ к элементам в середине
   - Нужна сортировка элементов
   - Важна гибкость в работе с данными

## Особенности реализации

```cpp
// Пример реализации stack
template<typename T, typename Container = std::deque<T>>
class Stack {
    Container c;
    
public:
    void push(const T& value) {
        c.push_back(value);
    }
    
    void pop() {
        c.pop_back();
    }
    
    T& top() {
        return c.back();
    }
    
    const T& top() const {
        return c.back();
    }
    
    bool empty() const {
        return c.empty();
    }
    
    size_t size() const {
        return c.size();
    }
};

// Пример реализации queue
template<typename T, typename Container = std::deque<T>>
class Queue {
    Container c;
    
public:
    void push(const T& value) {
        c.push_back(value);
    }
    
    void pop() {
        c.pop_front();
    }
    
    T& front() {
        return c.front();
    }
    
    const T& front() const {
        return c.front();
    }
    
    T& back() {
        return c.back();
    }
    
    const T& back() const {
        return c.back();
    }
    
    bool empty() const {
        return c.empty();
    }
    
    size_t size() const {
        return c.size();
    }
};
``` 