# Как использовать regex_match?

`std::regex_match` - это функция, которая проверяет, соответствует ли вся строка заданному регулярному выражению. В отличие от `regex_search`, она требует полного соответствия строки шаблону.

## Основные характеристики:
- Проверяет полное соответствие строки шаблону
- Возвращает `bool`
- Может сохранять найденные группы
- Поддерживает различные форматы вывода

## Примеры использования:

```cpp
// Правильное использование
std::regex email_pattern(R"([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})");
std::string email = "user@example.com";

// Простая проверка
if (std::regex_match(email, email_pattern)) {
    std::cout << "Valid email\n";
}

// Сохранение групп
std::regex date_pattern(R"((\d{2})/(\d{2})/(\d{4}))");
std::string date = "25/12/2023";
std::smatch matches;

if (std::regex_match(date, matches, date_pattern)) {
    std::cout << "Day: " << matches[1] << '\n';
    std::cout << "Month: " << matches[2] << '\n';
    std::cout << "Year: " << matches[3] << '\n';
}

// Использование с string_view
std::string_view date_view = "25/12/2023";
if (std::regex_match(date_view.begin(), date_view.end(), date_pattern)) {
    std::cout << "Valid date\n";
}
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern("\\d+");
std::string text = "123abc";  // Содержит не только цифры

// Ошибка: regex_match вернет false, так как строка не полностью соответствует шаблону
if (std::regex_match(text, pattern)) {
    // Этот код не выполнится
}

// Неправильная работа с группами
std::smatch matches;
std::regex_match("25/12/2023", matches, date_pattern);
std::cout << matches[4] << '\n';  // Ошибка: обращение к несуществующей группе

// Небезопасное использование
std::regex dangerous_pattern(".*");
std::string input = "Very long string...";
if (std::regex_match(input, dangerous_pattern)) {  // Может быть очень медленным
    // ...
}
```

## Важные замечания:
1. `regex_match` требует полного соответствия строки шаблону
2. Для поиска подстроки лучше использовать `regex_search`
3. Группы нумеруются с 1, группа 0 содержит полное совпадение
4. Важно проверять существование групп перед использованием
5. Рекомендуется использовать именованные группы для лучшей читаемости

## Производительность:
- `regex_match` работает быстрее `regex_search` для полных совпадений
- Сохранение групп требует дополнительных ресурсов
- Компиляция регулярного выражения выполняется один раз
- Сложные паттерны могут работать медленно 