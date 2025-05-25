# Как работает `priority_queue`?

## Ответ
`std::priority_queue` - это адаптер контейнера, который реализует очередь с приоритетом. Элементы в очереди автоматически сортируются, и элемент с наивысшим приоритетом всегда находится наверху.

Основные характеристики:
1. По умолчанию использует `std::vector` как базовый контейнер
2. По умолчанию использует `std::less` как функцию сравнения
3. Основные операции: push, pop, top
4. Вставка и удаление - O(log n)
5. Доступ к верхнему элементу - O(1)

## Примеры кода

### Правильное использование

```cpp
// Создание очереди с приоритетом (по умолчанию наибольший элемент сверху)
std::priority_queue<int> pq;

// Добавление элементов
pq.push(3);
pq.push(1);
pq.push(4);
pq.push(1);
pq.push(5);

// Получение верхнего элемента
int top = pq.top();  // 5

// Удаление верхнего элемента
pq.pop();  // Удаляет 5

// Проверка размера и пустоты
size_t size = pq.size();
bool empty = pq.empty();

// Создание очереди с приоритетом (наименьший элемент сверху)
std::priority_queue<int, std::vector<int>, std::greater<int>> min_pq;
min_pq.push(3);
min_pq.push(1);
min_pq.push(4);
min_pq.push(1);
min_pq.push(5);

// Получение наименьшего элемента
int min_top = min_pq.top();  // 1
```

### Неправильное использование

```cpp
// Неправильно: попытка прямого доступа к элементам
std::priority_queue<int> pq = {1, 2, 3};  // Ошибка! Нет конструктора от initializer_list
pq[0];  // Ошибка! Нет оператора []

// Правильно: последовательное добавление элементов
std::priority_queue<int> pq;
pq.push(1);
pq.push(2);
pq.push(3);

// Неправильно: использование priority_queue для частого доступа к элементам
std::priority_queue<int> pq;
for (int i = 0; i < 1000; ++i) {
    pq.push(i);
}
for (int i = 0; i < 1000; ++i) {
    int value = pq.top();  // Неэффективно для частого доступа
    pq.pop();
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

1. Используйте `std::priority_queue` когда:
   - Нужна очередь с приоритетом
   - Требуется только доступ к элементу с наивысшим приоритетом
   - Нужна автоматическая сортировка элементов
   - Важна эффективность операций push/pop

2. Не используйте `std::priority_queue` когда:
   - Нужен произвольный доступ к элементам
   - Требуется доступ к элементам в середине
   - Нужна гибкость в работе с данными
   - Важна эффективность при частом доступе к элементам

## Особенности реализации

```cpp
// Пример реализации priority_queue
template<typename T, typename Container = std::vector<T>,
         typename Compare = std::less<typename Container::value_type>>
class PriorityQueue {
    Container c;
    Compare comp;
    
    void heapify_up(size_t index) {
        while (index > 0) {
            size_t parent = (index - 1) / 2;
            if (comp(c[parent], c[index])) {
                std::swap(c[parent], c[index]);
                index = parent;
            } else {
                break;
            }
        }
    }
    
    void heapify_down(size_t index) {
        size_t left = 2 * index + 1;
        size_t right = 2 * index + 2;
        size_t largest = index;
        
        if (left < c.size() && comp(c[largest], c[left])) {
            largest = left;
        }
        
        if (right < c.size() && comp(c[largest], c[right])) {
            largest = right;
        }
        
        if (largest != index) {
            std::swap(c[index], c[largest]);
            heapify_down(largest);
        }
    }
    
public:
    void push(const T& value) {
        c.push_back(value);
        heapify_up(c.size() - 1);
    }
    
    void pop() {
        if (c.empty()) return;
        
        c[0] = c.back();
        c.pop_back();
        
        if (!c.empty()) {
            heapify_down(0);
        }
    }
    
    const T& top() const {
        return c.front();
    }
    
    bool empty() const {
        return c.empty();
    }
    
    size_t size() const {
        return c.size();
    }
};
``` 