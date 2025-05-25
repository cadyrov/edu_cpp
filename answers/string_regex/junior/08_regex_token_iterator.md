# Как использовать regex_token_iterator?

`std::regex_token_iterator` - это итератор, который позволяет перебирать либо совпадения регулярного выражения, либо части строки между совпадениями. Он особенно полезен для разбиения строки на токены.

## Основные характеристики:
- Может перебирать совпадения или части между совпадениями
- Поддерживает выбор конкретных групп
- Автоматически обновляет позицию
- Может работать с различными типами строк

## Примеры использования:

```cpp
// Правильное использование
std::regex pattern(R"(\s*,\s*)");  // Разделитель: запятая с пробелами
std::string text = "apple, banana, orange";

// Разбиение строки на токены
std::sregex_token_iterator it(text.begin(), text.end(), pattern, -1);
std::sregex_token_iterator end;

while (it != end) {
    std::cout << "Token: " << *it << '\n';
    ++it;
}

// Использование с группами
std::regex email_pattern(R"(([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+))");
std::string emails = "user1@example.com, user2@domain.com";

// Получение только имен пользователей
std::sregex_token_iterator email_it(emails.begin(), emails.end(), email_pattern, 1);
while (email_it != end) {
    std::cout << "Username: " << *email_it << '\n';
    ++email_it;
}

// Получение и совпадений, и частей между ними
std::vector<int> submatches = {-1, 0};  // -1 для частей между совпадениями, 0 для совпадений
std::sregex_token_iterator complex_it(text.begin(), text.end(), pattern, submatches);
while (complex_it != end) {
    std::cout << "Part: " << *complex_it << '\n';
    ++complex_it;
}
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern("\\d+");
std::string text = "abc123def456";

// Ошибка: итератор не обновляется
std::sregex_token_iterator it(text.begin(), text.end(), pattern, 0);
while (it != std::sregex_token_iterator()) {
    std::cout << *it << '\n';
    // Отсутствует ++it - бесконечный цикл
}

// Небезопасное использование
std::regex dangerous_pattern(".*");
std::string input = "Very long string...";
std::sregex_token_iterator dangerous_it(input.begin(), input.end(), dangerous_pattern, 0);
// Может быть очень медленным и потреблять много памяти

// Неправильная работа с группами
std::regex group_pattern(R"((\d+))");
std::string text2 = "123";
std::sregex_token_iterator group_it(text2.begin(), text2.end(), group_pattern, 2);
// Ошибка: группа 2 не существует
```

## Важные замечания:
1. Индекс -1 используется для получения частей между совпадениями
2. Индекс 0 используется для получения полных совпадений
3. Индексы 1 и выше используются для получения конкретных групп
4. Можно указать несколько индексов для получения разных частей
5. Итератор автоматически обновляет свою позицию

## Производительность:
- Создание итератора может быть дорогой операцией
- Перебор всех токенов может быть ресурсоемким
- Сложные шаблоны могут работать медленно
- Рекомендуется использовать более простые шаблоны, где это возможно 