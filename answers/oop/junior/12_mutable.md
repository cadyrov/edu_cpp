# Mutable

## Вопрос
Как использовать `mutable`?

## Ответ
`mutable` - это спецификатор в C++, который позволяет изменять член класса даже в const методах. Это полезно для членов, которые не влияют на логическое состояние объекта, но могут изменяться для оптимизации или кэширования.

Основные характеристики:
- Позволяет изменять член в const методах
- Не влияет на const корректность объекта
- Используется для кэширования и отложенных вычислений
- Применяется к членам, которые не являются частью логического состояния
- Помогает оптимизировать производительность
- Должен использоваться с осторожностью

## Примеры кода

### Правильный пример:
```cpp
class Cache {
private:
    std::vector<int> data;
    mutable bool isSorted;  // Флаг сортировки
    mutable std::vector<int> sortedData;  // Кэшированный отсортированный массив
    
public:
    Cache(const std::vector<int>& input) : data(input), isSorted(false) {}
    
    // Const метод, который может изменять mutable члены
    std::vector<int> getSortedData() const {
        if (!isSorted) {
            sortedData = data;  // Копируем данные
            std::sort(sortedData.begin(), sortedData.end());  // Сортируем
            isSorted = true;  // Обновляем флаг
        }
        return sortedData;
    }
    
    // Const метод с кэшированием
    int getMax() const {
        if (!isSorted) {
            getSortedData();  // Используем кэширование
        }
        return sortedData.back();  // Возвращаем максимальный элемент
    }
    
    // Не-const метод
    void addNumber(int num) {
        data.push_back(num);
        isSorted = false;  // Сбрасываем флаг при изменении данных
    }
};

// Пример использования
void example() {
    const Cache cache({5, 2, 8, 1, 9});
    
    // Первый вызов - сортировка
    auto sorted1 = cache.getSortedData();
    
    // Второй вызов - использование кэша
    auto sorted2 = cache.getSortedData();
    
    // Получение максимума с использованием кэша
    int max = cache.getMax();
}
```

### Неправильный пример:
```cpp
class BadCache {
private:
    std::vector<int> data;
    mutable int sum;  // Неправильное использование mutable
    mutable bool isDirty;  // Неправильное использование mutable
    
public:
    BadCache(const std::vector<int>& input) : data(input), sum(0), isDirty(true) {}
    
    // Неправильное использование mutable
    int getSum() const {
        if (isDirty) {
            sum = 0;
            for (int num : data) {
                sum += num;  // Вычисление суммы в const методе
            }
            isDirty = false;
        }
        return sum;
    }
    
    // Неправильное использование mutable
    void addNumber(int num) {
        data.push_back(num);
        sum += num;  // Обновление mutable члена
        isDirty = false;
    }
    
    // Неправильное использование mutable
    void clear() const {
        data.clear();  // Ошибка: попытка изменить не-mutable член
        sum = 0;
        isDirty = true;
    }
};

// Пример проблемного использования
void badExample() {
    const BadCache cache({1, 2, 3});
    
    int sum1 = cache.getSum();  // Вычисление суммы
    int sum2 = cache.getSum();  // Использование кэша
    
    // cache.clear();  // Ошибка компиляции
}
```

## Комментарии
- В правильном примере:
  - Обоснованное использование mutable
  - Правильное кэширование данных
  - Сохранение const корректности
  - Эффективное использование ресурсов
  - Четкое разделение логического состояния

- В неправильном примере:
  - Необоснованное использование mutable
  - Нарушение const корректности
  - Неправильное кэширование
  - Смешивание логического состояния
  - Нарушение принципов ООП 