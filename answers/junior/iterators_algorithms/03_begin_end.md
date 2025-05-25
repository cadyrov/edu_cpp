# Как работает begin() и end()?

## Вопрос
Как работает `begin()` и `end()`?

## Ответ
`begin()` и `end()` - это методы контейнеров STL, которые возвращают итераторы:
- `begin()` возвращает итератор на первый элемент контейнера
- `end()` возвращает итератор на позицию после последнего элемента (past-the-end)

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <list>
#include <iostream>

int main() {
    // Использование с vector
    std::vector<int> vec = {1, 2, 3, 4, 5};
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // Использование с list
    std::list<int> lst = {1, 2, 3, 4, 5};
    for (auto it = lst.begin(); it != lst.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // Использование с массивом
    int arr[] = {1, 2, 3, 4, 5};
    for (auto it = std::begin(arr); it != std::end(arr); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### Неправильное использование
```cpp
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    // ❌ Неправильно: разыменование end()
    auto it = vec.end();
    std::cout << *it << std::endl; // Неопределенное поведение
    
    // ❌ Неправильно: использование end() для пустого контейнера
    std::vector<int> empty_vec;
    if (empty_vec.begin() == empty_vec.end()) {
        std::cout << "Container is empty" << std::endl;
    }
    
    // ❌ Неправильно: изменение итератора end()
    auto it2 = vec.end();
    --it2; // Это нормально
    ++it2; // Это нормально
    it2 += 2; // Неопределенное поведение для не random-access итераторов
    
    return 0;
}
```

## Комментарии
1. `begin()` и `end()` работают для всех контейнеров STL
2. Для массивов в стиле C можно использовать `std::begin()` и `std::end()`
3. `end()` указывает на позицию после последнего элемента
4. Разыменование `end()` приводит к неопределенному поведению
5. Для пустого контейнера `begin() == end()`
6. В современном C++ предпочтительнее использовать range-based for loop
7. Существуют также `cbegin()/cend()` для константных итераторов
8. Для обратного обхода используются `rbegin()/rend()` 