# Как использовать binary_search()?

## Вопрос
Как использовать `binary_search()`?

## Ответ
`std::binary_search()` - это алгоритм STL, который проверяет, существует ли элемент в отсортированном диапазоне. Использует бинарный поиск, поэтому требует, чтобы диапазон был отсортирован. Возвращает `true`, если элемент найден, и `false` в противном случае.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    // Поиск в отсортированном vector
    std::vector<int> vec = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    
    // Поиск существующего элемента
    bool found = std::binary_search(vec.begin(), vec.end(), 5);
    std::cout << "5 found: " << found << std::endl; // Выведет: true
    
    // Поиск несуществующего элемента
    found = std::binary_search(vec.begin(), vec.end(), 10);
    std::cout << "10 found: " << found << std::endl; // Выведет: false
    
    // Поиск с пользовательским компаратором
    std::vector<int> vec2 = {9, 8, 7, 6, 5, 4, 3, 2, 1}; // Отсортирован по убыванию
    found = std::binary_search(vec2.begin(), vec2.end(), 5, std::greater<int>());
    std::cout << "5 found in descending order: " << found << std::endl; // Выведет: true
    
    // Поиск в части контейнера
    found = std::binary_search(vec.begin() + 1, vec.end() - 1, 5);
    std::cout << "5 found in range: " << found << std::endl; // Выведет: true
    
    // Поиск объектов
    struct Point {
        int x, y;
        bool operator<(const Point& other) const {
            return x < other.x || (x == other.x && y < other.y);
        }
    };
    
    std::vector<Point> points = {{1,1}, {1,2}, {2,1}, {2,2}};
    Point p{1,2};
    found = std::binary_search(points.begin(), points.end(), p);
    std::cout << "Point (1,2) found: " << found << std::endl; // Выведет: true

    return 0;
}
```

### Неправильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    // ❌ Неправильно: поиск в неотсортированном контейнере
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    bool found = std::binary_search(vec.begin(), vec.end(), 5); // Неопределенное поведение
    
    // ❌ Неправильно: использование инвалидированного итератора
    std::vector<int> vec2 = {1, 2, 3, 4, 5};
    auto it = vec2.begin();
    vec2.push_back(6); // Инвалидирует итератор
    found = std::binary_search(it, vec2.end(), 3); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    found = std::binary_search(vec2.end(), vec2.begin(), 3); // Неопределенное поведение
    
    // ❌ Неправильно: использование несовместимого компаратора
    std::vector<int> vec3 = {1, 2, 3, 4, 5};
    found = std::binary_search(vec3.begin(), vec3.end(), 3,
        [](int a, int b) { return a > b; }); // Несовместимый компаратор
    
    return 0;
}
```

## Комментарии
1. `binary_search()` требует итераторы, удовлетворяющие требованиям RandomAccessIterator
2. Сложность алгоритма - O(log n)
3. Диапазон должен быть отсортирован перед использованием
4. Для пользовательских типов должен быть определен оператор `<` или предоставлен компаратор
5. Компаратор должен быть строгим слабым порядком
6. Компаратор не должен изменять состояние
7. В современном C++ можно использовать `std::ranges::binary_search` для более удобного синтаксиса
8. Алгоритм не возвращает позицию найденного элемента
9. Для получения позиции элемента используйте `std::lower_bound` или `std::upper_bound` 