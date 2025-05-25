# Что такое regex_iterator?

`std::regex_iterator` - это итератор, который позволяет последовательно перебирать все вхождения регулярного выражения в строке. Он автоматически обновляет свою позицию после каждого совпадения.

## Основные характеристики:
- Перебирает все совпадения в строке
- Автоматически обновляет позицию
- Поддерживает стандартные операции итератора
- Может работать с различными типами строк

## Примеры использования:

```cpp
// Правильное использование
std::regex pattern(R"(\d{3}-\d{3}-\d{4})");
std::string text = "Phone: 555-123-4567, Fax: 555-987-6543";

// Перебор всех совпадений
std::sregex_iterator it(text.begin(), text.end(), pattern);
std::sregex_iterator end;

while (it != end) {
    std::cout << "Found: " << it->str() << '\n';
    ++it;
}

// Использование с группами
std::regex email_pattern(R"(([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+))");
std::string emails = "user1@example.com, user2@domain.com";

std::sregex_iterator email_it(emails.begin(), emails.end(), email_pattern);
while (email_it != end) {
    std::cout << "Username: " << (*email_it)[1] << '\n';
    std::cout << "Domain: " << (*email_it)[2] << '\n';
    ++email_it;
}

// Использование с string_view
std::string_view text_view = "Phone: 555-123-4567";
std::cregex_iterator view_it(text_view.begin(), text_view.end(), pattern);
while (view_it != end) {
    std::cout << "Found: " << view_it->str() << '\n';
    ++view_it;
}
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern("\\d+");
std::string text = "abc123def456";

// Ошибка: итератор не обновляется
std::sregex_iterator it(text.begin(), text.end(), pattern);
while (it != std::sregex_iterator()) {
    std::cout << it->str() << '\n';
    // Отсутствует ++it - бесконечный цикл
}

// Небезопасное использование
std::regex dangerous_pattern(".*");
std::string input = "Very long string...";
std::sregex_iterator dangerous_it(input.begin(), input.end(), dangerous_pattern);
// Может быть очень медленным и потреблять много памяти

// Неправильная работа с итератором
std::sregex_iterator it2(text.begin(), text.end(), pattern);
std::cout << (it2 + 1)->str() << '\n';  // Ошибка: оператор + не определен
```

## Важные замечания:
1. Итератор автоматически обновляет свою позицию
2. Важно не забывать инкрементировать итератор
3. Итератор становится недействительным при изменении исходной строки
4. Поддерживает различные типы строк (string, string_view, wstring)
5. Можно использовать с различными типами регулярных выражений

## Производительность:
- Создание итератора может быть дорогой операцией
- Перебор всех совпадений может быть ресурсоемким
- Сложные шаблоны могут работать медленно
- Рекомендуется использовать более простые шаблоны, где это возможно 