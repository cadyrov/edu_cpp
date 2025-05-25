# Что такое count_if()?

## Вопрос
Что такое `count_if()`?

## Ответ
`std::count_if()` - это алгоритм STL, который подсчитывает количество элементов в диапазоне, удовлетворяющих заданному предикату (условию). Возвращает количество элементов, для которых предикат возвращает true.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <list>
#include <algorithm>
#include <iostream>

int main() {
    // Подсчет четных чисел
    std::vector<int> vec = {1, 2, 3, 4, 5, 6, 7, 8};
    int count = std::count_if(vec.begin(), vec.end(),
        [](int n) { return n % 2 == 0; });
    std::cout << "Number of even numbers: " << count << std::endl; // Выведет: 4

    // Подсчет чисел больше 5
    count = std::count_if(vec.begin(), vec.end(),
        [](int n) { return n > 5; });
    std::cout << "Numbers greater than 5: " << count << std::endl; // Выведет: 3

    // Использование с функтором
    struct IsPositive {
        bool operator()(int n) const { return n > 0; }
    };
    count = std::count_if(vec.begin(), vec.end(), IsPositive());
    std::cout << "Positive numbers: " << count << std::endl; // Выведет: 8

    // Подсчет в части контейнера
    count = std::count_if(vec.begin() + 1, vec.end() - 1,
        [](int n) { return n % 2 == 0; });
    std::cout << "Even numbers in range: " << count << std::endl; // Выведет: 3

    // Подсчет объектов по условию
    struct Point { int x, y; };
    std::vector<Point> points = {{1,1}, {2,2}, {3,3}, {4,4}, {5,5}};
    count = std::count_if(points.begin(), points.end(),
        [](const Point& p) { return p.x == p.y; });
    std::cout << "Points with equal coordinates: " << count << std::endl; // Выведет: 5

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
    
    // ❌ Неправильно: предикат изменяет состояние
    int sum = 0;
    auto count = std::count_if(vec.begin(), vec.end(),
        [&sum](int n) { 
            sum += n; // Предикат не должен иметь побочных эффектов
            return n > 3; 
        });
    
    // ❌ Неправильно: предикат выбрасывает исключение
    auto count2 = std::count_if(vec.begin(), vec.end(),
        [](int n) { 
            if (n == 0) throw std::runtime_error("Zero found");
            return n > 3; 
        });
    
    // ❌ Неправильно: использование инвалидированного итератора
    auto it = vec.begin();
    vec.push_back(6); // Инвалидирует итератор
    auto count3 = std::count_if(it, vec.end(),
        [](int n) { return n > 3; }); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    auto count4 = std::count_if(vec.end(), vec.begin(),
        [](int n) { return n > 3; }); // Неопределенное поведение
    
    return 0;
}
```

## Комментарии
1. `count_if()` работает с любыми итераторами, удовлетворяющими требованиям InputIterator
2. Сложность алгоритма - O(n), где n - размер диапазона
3. Предикат должен быть функцией или функциональным объектом, возвращающим bool
4. Предикат не должен изменять состояние (должен быть чистым)
5. Предикат не должен выбрасывать исключения
6. Можно использовать лямбда-функции, функторы или обычные функции
7. В современном C++ можно использовать `std::ranges::count_if` для более удобного синтаксиса
8. Алгоритм не изменяет элементы контейнера
9. Полезен для подсчета элементов, удовлетворяющих сложным условиям 