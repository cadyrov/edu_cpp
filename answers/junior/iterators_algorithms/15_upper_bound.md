# Как использовать upper_bound()?

## Вопрос
Как использовать `upper_bound()`?

## Ответ
`std::upper_bound()` - это алгоритм STL, который находит первый элемент в отсортированном диапазоне, который больше заданного значения. Возвращает итератор на найденный элемент или на конец диапазона, если такого элемента нет. Использует бинарный поиск, поэтому требует, чтобы диапазон был отсортирован.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    // Поиск в отсортированном vector
    std::vector<int> vec = {1, 2, 3, 3, 3, 4, 5, 6, 7, 8, 9};
    
    // Поиск первого элемента > 3
    auto it = std::upper_bound(vec.begin(), vec.end(), 3);
    std::cout << "First element > 3: " << *it << std::endl; // Выведет: 4
    
    // Поиск первого элемента > 9 (не существует)
    it = std::upper_bound(vec.begin(), vec.end(), 9);
    if (it == vec.end()) {
        std::cout << "No element > 9 found" << std::endl; // Выведет это
    }
    
    // Поиск с пользовательским компаратором
    std::vector<int> vec2 = {9, 8, 7, 6, 5, 4, 3, 2, 1}; // Отсортирован по убыванию
    it = std::upper_bound(vec2.begin(), vec2.end(), 5, std::greater<int>());
    std::cout << "First element < 5: " << *it << std::endl; // Выведет: 4
    
    // Поиск в части контейнера
    it = std::upper_bound(vec.begin() + 1, vec.end() - 1, 3);
    std::cout << "First element > 3 in range: " << *it << std::endl; // Выведет: 4
    
    // Поиск объектов
    struct Point {
        int x, y;
        bool operator<(const Point& other) const {
            return x < other.x || (x == other.x && y < other.y);
        }
    };
    
    std::vector<Point> points = {{1,1}, {1,2}, {2,1}, {2,2}};
    Point p{1,2};
    auto point_it = std::upper_bound(points.begin(), points.end(), p);
    std::cout << "First point > (1,2): (" << point_it->x << "," << point_it->y << ")" << std::endl; // Выведет: (2,1)

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
    auto it = std::upper_bound(vec.begin(), vec.end(), 5); // Неопределенное поведение
    
    // ❌ Неправильно: использование инвалидированного итератора
    std::vector<int> vec2 = {1, 2, 3, 4, 5};
    auto it2 = vec2.begin();
    vec2.push_back(6); // Инвалидирует итератор
    it = std::upper_bound(it2, vec2.end(), 3); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    it = std::upper_bound(vec2.end(), vec2.begin(), 3); // Неопределенное поведение
    
    // ❌ Неправильно: использование несовместимого компаратора
    std::vector<int> vec3 = {1, 2, 3, 4, 5};
    it = std::upper_bound(vec3.begin(), vec3.end(), 3,
        [](int a, int b) { return a > b; }); // Несовместимый компаратор
    
    return 0;
}
```

## Комментарии
1. `upper_bound()` требует итераторы, удовлетворяющие требованиям RandomAccessIterator
2. Сложность алгоритма - O(log n)
3. Диапазон должен быть отсортирован перед использованием
4. Для пользовательских типов должен быть определен оператор `<` или предоставлен компаратор
5. Компаратор должен быть строгим слабым порядком
6. Компаратор не должен изменять состояние
7. В современном C++ можно использовать `std::ranges::upper_bound` для более удобного синтаксиса
8. Алгоритм возвращает итератор на первый элемент, который больше заданного значения
9. Если элемент не найден, возвращается итератор на конец диапазона
10. Для поиска первого элемента, который не меньше заданного значения, используйте `std::lower_bound`
11. Разница между `upper_bound` и `lower_bound` позволяет найти диапазон равных элементов 