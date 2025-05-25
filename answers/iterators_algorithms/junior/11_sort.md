# Как работает sort()?

## Вопрос
Как работает `sort()`?

## Ответ
`std::sort()` - это алгоритм STL, который сортирует элементы в диапазоне в порядке возрастания. По умолчанию использует оператор `<` для сравнения элементов, но может принимать пользовательский компаратор.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    // Сортировка vector
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    std::sort(vec.begin(), vec.end());
    for (int n : vec) {
        std::cout << n << " "; // Выведет: 1 2 3 5 8 9
    }
    std::cout << std::endl;

    // Сортировка с пользовательским компаратором
    std::sort(vec.begin(), vec.end(), std::greater<int>());
    for (int n : vec) {
        std::cout << n << " "; // Выведет: 9 8 5 3 2 1
    }
    std::cout << std::endl;

    // Сортировка части контейнера
    std::vector<int> vec2 = {5, 2, 8, 1, 9, 3};
    std::sort(vec2.begin() + 1, vec2.end() - 1);
    for (int n : vec2) {
        std::cout << n << " "; // Выведет: 5 1 2 3 8 9
    }
    std::cout << std::endl;

    // Сортировка объектов
    struct Point {
        int x, y;
        bool operator<(const Point& other) const {
            return x < other.x || (x == other.x && y < other.y);
        }
    };
    std::vector<Point> points = {{2,3}, {1,2}, {2,1}, {1,1}};
    std::sort(points.begin(), points.end());
    for (const auto& p : points) {
        std::cout << "(" << p.x << "," << p.y << ") "; // Выведет: (1,1) (1,2) (2,1) (2,3)
    }
    std::cout << std::endl;

    return 0;
}
```

### Неправильное использование
```cpp
#include <vector>
#include <list>
#include <algorithm>
#include <iostream>

int main() {
    // ❌ Неправильно: сортировка list
    std::list<int> lst = {5, 2, 8, 1, 9, 3};
    std::sort(lst.begin(), lst.end()); // Ошибка компиляции: list не поддерживает random access
    
    // ❌ Неправильно: использование инвалидированного итератора
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    auto it = vec.begin();
    vec.push_back(6); // Инвалидирует итератор
    std::sort(it, vec.end()); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    std::sort(vec.end(), vec.begin()); // Неопределенное поведение
    
    // ❌ Неправильно: нестабильный компаратор
    std::sort(vec.begin(), vec.end(),
        [](int a, int b) { 
            static int count = 0;
            return (count++ % 2) == 0 ? a < b : b < a; // Нестабильный компаратор
        });
    
    return 0;
}
```

## Комментарии
1. `sort()` требует итераторы, удовлетворяющие требованиям RandomAccessIterator
2. Сложность алгоритма - O(n log n)
3. Алгоритм нестабильный (не сохраняет относительный порядок равных элементов)
4. Для пользовательских типов должен быть определен оператор `<` или предоставлен компаратор
5. Компаратор должен быть строгим слабым порядком
6. Компаратор не должен изменять состояние
7. В современном C++ можно использовать `std::ranges::sort` для более удобного синтаксиса
8. Для стабильной сортировки используется `std::stable_sort`
9. Для частичной сортировки используется `std::partial_sort` 