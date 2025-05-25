# Какие типы итераторов есть в STL?

## Вопрос
Какие типы итераторов есть в STL?

## Ответ
В STL существует 5 основных категорий итераторов, расположенных в иерархии от наиболее ограниченных к наиболее функциональным:

1. Input Iterator - может только читать элементы
2. Output Iterator - может только записывать элементы
3. Forward Iterator - может читать и записывать элементы, двигаться только вперед
4. Bidirectional Iterator - может двигаться в обоих направлениях
5. Random Access Iterator - может перемещаться на произвольное расстояние

## Примеры кода

### Правильное использование
```cpp
#include <vector>
#include <list>
#include <forward_list>
#include <iostream>

int main() {
    // Random Access Iterator (vector)
    std::vector<int> vec = {1, 2, 3, 4, 5};
    auto it1 = vec.begin();
    it1 += 2; // Можно перемещаться на произвольное расстояние
    std::cout << *it1 << std::endl; // 3

    // Bidirectional Iterator (list)
    std::list<int> lst = {1, 2, 3, 4, 5};
    auto it2 = lst.begin();
    ++it2; // Вперед
    --it2; // Назад
    std::cout << *it2 << std::endl; // 1

    // Forward Iterator (forward_list)
    std::forward_list<int> flst = {1, 2, 3, 4, 5};
    auto it3 = flst.begin();
    ++it3; // Только вперед
    std::cout << *it3 << std::endl; // 2

    return 0;
}
```

### Неправильное использование
```cpp
#include <list>
#include <forward_list>
#include <iostream>

int main() {
    // ❌ Неправильно: попытка использовать random access с list
    std::list<int> lst = {1, 2, 3, 4, 5};
    auto it1 = lst.begin();
    it1 += 2; // Ошибка компиляции: list не поддерживает random access

    // ❌ Неправильно: попытка двигаться назад в forward_list
    std::forward_list<int> flst = {1, 2, 3, 4, 5};
    auto it2 = flst.begin();
    ++it2;
    --it2; // Ошибка компиляции: forward_list не поддерживает движение назад

    return 0;
}
```

## Комментарии
1. Каждый тип итератора поддерживает определенный набор операций
2. Контейнеры предоставляют итераторы определенного типа:
   - vector, array, deque - Random Access Iterator
   - list, set, map - Bidirectional Iterator
   - forward_list - Forward Iterator
   - istream_iterator - Input Iterator
   - ostream_iterator - Output Iterator
3. Алгоритмы STL требуют итераторы определенного типа
4. При написании шаблонных функций важно указывать минимальные требования к итераторам
5. В современном C++ можно использовать концепты для проверки типа итератора 