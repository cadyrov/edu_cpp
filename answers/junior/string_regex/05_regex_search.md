# Что такое regex_search?

`std::regex_search` - это функция, которая ищет первое вхождение подстроки, соответствующей регулярному выражению. В отличие от `regex_match`, она не требует полного соответствия строки шаблону.

## Основные характеристики:
- Ищет первое вхождение шаблона в строке
- Возвращает `bool`
- Может сохранять найденные группы
- Поддерживает итерацию по всем совпадениям

## Примеры использования:

```cpp
// Правильное использование
std::regex phone_pattern(R"(\d{3}-\d{3}-\d{4})");
std::string text = "Contact us at 555-123-4567 or 555-987-6543";

// Поиск первого совпадения
std::smatch matches;
if (std::regex_search(text, matches, phone_pattern)) {
    std::cout << "Found phone: " << matches[0] << '\n';
}

// Поиск всех совпадений
std::string::const_iterator searchStart(text.cbegin());
while (std::regex_search(searchStart, text.cend(), matches, phone_pattern)) {
    std::cout << "Found phone: " << matches[0] << '\n';
    searchStart = matches.suffix().first;
}

// Использование с группами
std::regex email_pattern(R"(([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+))");
std::string email = "user@example.com";
if (std::regex_search(email, matches, email_pattern)) {
    std::cout << "Username: " << matches[1] << '\n';
    std::cout << "Domain: " << matches[2] << '\n';
}
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern("\\d+");
std::string text = "abc123def";

// Ошибка: regex_search найдет только первое совпадение
if (std::regex_search(text, pattern)) {
    // Найдет только "123"
}

// Неправильная работа с итераторами
std::string::const_iterator it = text.begin();
while (std::regex_search(it, text.end(), matches, pattern)) {
    it = matches[0].first;  // Ошибка: может привести к бесконечному циклу
}

// Небезопасное использование
std::regex dangerous_pattern(".*");
std::string input = "Very long string...";
if (std::regex_search(input, dangerous_pattern)) {  // Может быть очень медленным
    // ...
}
```

## Важные замечания:
1. `regex_search` находит только первое совпадение
2. Для поиска всех совпадений нужно использовать цикл
3. Важно правильно обновлять итератор поиска
4. Группы нумеруются с 1, группа 0 содержит полное совпадение
5. Рекомендуется использовать именованные группы для лучшей читаемости

## Производительность:
- `regex_search` работает медленнее `regex_match` для полных совпадений
- Поиск всех совпадений может быть ресурсоемким
- Сохранение групп требует дополнительных ресурсов
- Сложные паттерны могут работать экспоненциально медленно 