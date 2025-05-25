# Рефакторинг и уменьшение сложности с помощью функций

## Почему важны функции и рефакторинг
- По мере роста программы функция main становится слишком длинной и сложной.
- Много переменных и блоков кода затрудняют понимание и сопровождение.
- Комментарии могут устаревать, а переменные — терять смысл.
- Лучший способ справиться со сложностью — разбивать код на небольшие функции, каждая из которых решает одну задачу.

## Что такое рефакторинг
- Рефакторинг — изменение структуры программы без изменения её функциональности.
- Цели рефакторинга:
  - Улучшить читаемость и поддержку кода
  - Упростить тестирование
  - Уменьшить размер отдельных частей программы
  - Повысить производительность
- Примеры рефакторинга: переименование переменных, выделение функций, устранение дублирования кода.

## Пример: подсчёт людей, достигших возраста
### До рефакторинга
```cpp
#include <iostream>
#include <vector>

int main() {
    int num_people;
    std::cin >> num_people;
    std::vector<int> ages;
    for (int i = 0; i < num_people; ++i) {
        int age;
        std::cin >> age;
        ages.push_back(age);
    }
    int min_age;
    std::cin >> min_age;
    int people_reached_min_age = 0;
    for (int age : ages) {
        if (age >= min_age) {
            ++people_reached_min_age;
        }
    }
    std::cout << people_reached_min_age << std::endl;
}
```

### После рефакторинга
- Выделяем универсальные функции для повторяющихся задач.
- Даем функциям и переменным осмысленные имена.

```cpp
int ReadInt() {
    int n;
    std::cin >> n;
    return n;
}

std::vector<int> ReadInts(int size) {
    std::vector<int> numbers;
    for (int i = 0; i < size; ++i) {
        numbers.push_back(ReadInt());
    }
    return numbers;
}

int CountItemsGreaterOrEqual(const std::vector<int>& numbers, int value) {
    int counter = 0;
    for (int number : numbers) {
        if (number >= value) {
            ++counter;
        }
    }
    return counter;
}

int main() {
    const int num_people = ReadInt();
    const std::vector<int> people_ages = ReadInts(num_people);
    const int min_age = ReadInt();
    const int people_reached_min_age = CountItemsGreaterOrEqual(people_ages, min_age);
    std::cout << people_reached_min_age << std::endl;
}
```

## Важные выводы
- main становится короче и понятнее
- Функции можно переиспользовать в других задачах
- Переменные можно делать константными, если они не изменяются
- Код становится самодокументируемым
- Тестировать отдельные функции проще

## Тестирование функций с помощью assert
- Для проверки корректности функций используйте макрос assert из <cassert>
- assert завершает программу, если условие ложно

```cpp
#include <cassert>
#include <vector>

int CountItemsGreaterOrEqual(const std::vector<int>& numbers, int value) {
    // ...
}

int main() {
    assert(CountItemsGreaterOrEqual({}, 42) == 0);
    const std::vector<int> numbers{5, 2, 8, 3, 4, 5};
    assert(CountItemsGreaterOrEqual(numbers, 2) == 6);
    assert(CountItemsGreaterOrEqual(numbers, 4) == 4);
    assert(CountItemsGreaterOrEqual(numbers, 8) == 1);
    assert(CountItemsGreaterOrEqual(numbers, 9) == 0);
}
```

## Для чего разбивают программу на функции?
- Для выделения смысловых блоков
- Для повторного использования кода
- Для облегчения тестирования
- Для самодокументирования кода
- Для уменьшения размера отдельных частей программы

## Важно помнить
- Рефакторинг не должен менять функциональность
- Хорошие имена функций и переменных делают код понятнее
- Универсальные функции легче переиспользовать
- Тестируйте функции по мере их написания 