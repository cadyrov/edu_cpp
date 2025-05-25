# Что такое stable_sort()?

## Вопрос
Что такое `stable_sort()`?

## Ответ
`std::stable_sort()` - это алгоритм STL, который сортирует элементы в диапазоне, сохраняя относительный порядок равных элементов. По умолчанию использует оператор `<` для сравнения, но может принимать пользовательский компаратор.

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    // Сортировка с сохранением порядка равных элементов
    struct Student {
        int grade;
        std::string name;
        bool operator<(const Student& other) const {
            return grade < other.grade;
        }
    };

    std::vector<Student> students = {
        {85, "Alice"},
        {92, "Bob"},
        {85, "Charlie"},
        {78, "David"},
        {92, "Eve"}
    };

    // Сортировка по оценкам
    std::stable_sort(students.begin(), students.end());
    
    for (const auto& s : students) {
        std::cout << s.grade << ": " << s.name << std::endl;
    }
    // Выведет:
    // 78: David
    // 85: Alice
    // 85: Charlie
    // 92: Bob
    // 92: Eve

    // Сортировка с пользовательским компаратором
    std::stable_sort(students.begin(), students.end(),
        [](const Student& a, const Student& b) {
            return a.grade > b.grade; // Сортировка по убыванию
        });

    for (const auto& s : students) {
        std::cout << s.grade << ": " << s.name << std::endl;
    }
    // Выведет:
    // 92: Bob
    // 92: Eve
    // 85: Alice
    // 85: Charlie
    // 78: David

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
    std::stable_sort(lst.begin(), lst.end()); // Ошибка компиляции: list не поддерживает random access
    
    // ❌ Неправильно: использование инвалидированного итератора
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    auto it = vec.begin();
    vec.push_back(6); // Инвалидирует итератор
    std::stable_sort(it, vec.end()); // Неопределенное поведение
    
    // ❌ Неправильно: неправильный порядок итераторов
    std::stable_sort(vec.end(), vec.begin()); // Неопределенное поведение
    
    // ❌ Неправильно: нестабильный компаратор
    std::stable_sort(vec.begin(), vec.end(),
        [](int a, int b) { 
            static int count = 0;
            return (count++ % 2) == 0 ? a < b : b < a; // Нестабильный компаратор
        });
    
    return 0;
}
```

## Комментарии
1. `stable_sort()` требует итераторы, удовлетворяющие требованиям RandomAccessIterator
2. Сложность алгоритма - O(n log n) в среднем случае
3. Алгоритм стабильный (сохраняет относительный порядок равных элементов)
4. Для пользовательских типов должен быть определен оператор `<` или предоставлен компаратор
5. Компаратор должен быть строгим слабым порядком
6. Компаратор не должен изменять состояние
7. В современном C++ можно использовать `std::ranges::stable_sort` для более удобного синтаксиса
8. `stable_sort()` обычно медленнее `sort()`, но гарантирует стабильность
9. Полезен, когда важен порядок равных элементов 