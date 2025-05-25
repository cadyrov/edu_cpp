# Что такое find_if()?

## Вопрос
Что такое `find_if()`?

## Ответ
`std::find_if()` - это алгоритм STL, который ищет первый элемент в диапазоне, удовлетворяющий заданному предикату (условию). Он возвращает итератор на найденный элемент или `end()` итератор, если такой элемент не найден.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <list>
#include <algorithm>
#include <iostream>

int main() {
    // Поиск первого четного числа
    std::vector<int> vec = {1, 3, 5, 2, 4, 6};
    auto it = std::find_if(vec.begin(), vec.end(), 
        [](int n) { return n % 2 == 0; });
    if (it != vec.end()) {
        std::cout << "First even number: " << *it << std::endl;
    }

    // Поиск первого числа больше 3
    auto it2 = std::find_if(vec.begin(), vec.end(), 
        [](int n) { return n > 3; });
    if (it2 != vec.end()) {
        std::cout << "First number > 3: " << *it2 << std::endl;
    }

    // Использование с функтором
    struct IsPositive {
        bool operator()(int n) const { return n > 0; }
    };
    auto it3 = std::find_if(vec.begin(), vec.end(), IsPositive());
    if (it3 != vec.end()) {
        std::cout << "First positive number: " << *it3 << std::endl;
    }

    // Поиск в части контейнера
    auto it4 = std::find_if(vec.begin() + 1, vec.end() - 1,
        [](int n) { return n % 2 == 0; });
    if (it4 != vec.end() - 1) {
        std::cout << "First even in range: " << *it4 << std::endl;
    }

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
    auto it = std::find_if(vec.begin(), vec.end(),
        [&sum](int n) { 
            sum += n; // Предикат не должен иметь побочных эффектов
            return n > 3; 
        });
    
    // ❌ Неправильно: предикат выбрасывает исключение
    auto it2 = std::find_if(vec.begin(), vec.end(),
        [](int n) { 
            if (n == 0) throw std::runtime_error("Zero found");
            return n > 3; 
        });
    
    // ❌ Неправильно: использование инвалидированного итератора
    auto it3 = vec.begin();
    vec.push_back(6); // Инвалидирует итератор
    auto result = std::find_if(it3, vec.end(),
        [](int n) { return n > 3; }); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    auto it4 = std::find_if(vec.end(), vec.begin(),
        [](int n) { return n > 3; }); // Неопределенное поведение
    
    return 0;
}
```

## Комментарии
1. `find_if()` работает с любыми итераторами, удовлетворяющими требованиям InputIterator
2. Сложность алгоритма - O(n), где n - размер диапазона
3. Предикат должен быть функцией или функциональным объектом, возвращающим bool
4. Предикат не должен изменять состояние (должен быть чистым)
5. Предикат не должен выбрасывать исключения
6. Можно использовать лямбда-функции, функторы или обычные функции
7. В современном C++ можно использовать `std::ranges::find_if` для более удобного синтаксиса
8. Для поиска последнего элемента, удовлетворяющего условию, используется `std::find_if_not`
9. Для поиска первого элемента, не удовлетворяющего условию, используется `std::find_if_not` 