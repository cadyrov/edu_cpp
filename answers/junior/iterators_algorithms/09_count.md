# Как использовать count()?

## Вопрос
Как использовать `count()`?

## Ответ
`std::count()` - это алгоритм STL, который подсчитывает количество элементов в диапазоне, равных заданному значению. Возвращает количество найденных элементов.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <list>
#include <algorithm>
#include <iostream>

int main() {
    // Подсчет в vector
    std::vector<int> vec = {1, 2, 3, 2, 4, 2, 5};
    int count = std::count(vec.begin(), vec.end(), 2);
    std::cout << "Number of 2's: " << count << std::endl; // Выведет: 3

    // Подсчет в list
    std::list<int> lst = {1, 2, 3, 2, 4, 2, 5};
    count = std::count(lst.begin(), lst.end(), 2);
    std::cout << "Number of 2's in list: " << count << std::endl; // Выведет: 3

    // Подсчет в части контейнера
    count = std::count(vec.begin() + 1, vec.end() - 1, 2);
    std::cout << "Number of 2's in range: " << count << std::endl; // Выведет: 2

    // Подсчет с использованием константных итераторов
    const std::vector<int> const_vec = {1, 2, 3, 2, 4, 2, 5};
    count = std::count(const_vec.cbegin(), const_vec.cend(), 2);
    std::cout << "Number of 2's in const: " << count << std::endl; // Выведет: 3

    // Подсчет объектов
    struct Point { int x, y; };
    std::vector<Point> points = {{1,1}, {2,2}, {1,1}, {3,3}, {1,1}};
    Point p{1,1};
    count = std::count(points.begin(), points.end(), p); // Требуется operator==
    std::cout << "Number of points (1,1): " << count << std::endl; // Выведет: 3

    return 0;
}
```

### Неправильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    // ❌ Неправильно: использование инвалидированного итератора
    auto it = vec.begin();
    vec.push_back(6); // Инвалидирует итератор
    int count = std::count(it, vec.end(), 2); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    count = std::count(vec.end(), vec.begin(), 2); // Неопределенное поведение
    
    // ❌ Неправильно: использование итераторов из разных контейнеров
    std::vector<int> vec2 = {1, 2, 3, 4, 5};
    count = std::count(vec.begin(), vec2.end(), 2); // Ошибка компиляции
    
    // ❌ Неправильно: использование с типом без operator==
    struct NoEquality { int x; };
    std::vector<NoEquality> no_eq = {{1}, {2}, {1}};
    NoEquality n{1};
    count = std::count(no_eq.begin(), no_eq.end(), n); // Ошибка компиляции
    
    return 0;
}
```

## Комментарии
1. `count()` работает с любыми итераторами, удовлетворяющими требованиям InputIterator
2. Сложность алгоритма - O(n), где n - размер диапазона
3. Для пользовательских типов должен быть определен оператор `==`
4. Возвращает количество элементов, равных заданному значению
5. Можно считать в части контейнера, указав другие итераторы
6. В современном C++ можно использовать `std::ranges::count` для более удобного синтаксиса
7. Для подсчета элементов, удовлетворяющих условию, используется `std::count_if`
8. Алгоритм не изменяет элементы контейнера
9. Работает с любыми типами, для которых определен оператор `==` 