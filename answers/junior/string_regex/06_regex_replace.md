# Как работает regex_replace?

`std::regex_replace` - это функция, которая заменяет все вхождения подстрок, соответствующих регулярному выражению, на указанную строку замены. Поддерживает различные форматы замены и флаги.

## Основные характеристики:
- Заменяет все вхождения шаблона
- Поддерживает ссылки на группы в строке замены
- Возвращает новую строку
- Поддерживает различные флаги форматирования

## Примеры использования:

```cpp
// Правильное использование
std::regex pattern(R"(\d{3}-\d{2}-\d{4})");
std::string text = "SSN: 123-45-6789, Another SSN: 987-65-4321";

// Простая замена
std::string masked = std::regex_replace(text, pattern, "XXX-XX-XXXX");
std::cout << masked << '\n';  // SSN: XXX-XX-XXXX, Another SSN: XXX-XX-XXXX

// Замена с использованием групп
std::regex email_pattern(R"(([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+))");
std::string email = "user@example.com";
std::string masked_email = std::regex_replace(email, email_pattern, "$1@***.$2");
std::cout << masked_email << '\n';  // user@***.com

// Использование флагов
std::string result = std::regex_replace(text, pattern, "XXX-XX-XXXX",
    std::regex_constants::format_first_only);  // Заменяет только первое вхождение
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern("\\d+");
std::string text = "123abc456";

// Ошибка: неправильный формат замены
std::string result = std::regex_replace(text, pattern, "$1");  // $1 не существует

// Небезопасное использование
std::regex dangerous_pattern(".*");
std::string input = "Very long string...";
std::string output = std::regex_replace(input, dangerous_pattern, "replacement");
// Может быть очень медленным и потреблять много памяти

// Неправильная работа с группами
std::regex group_pattern(R"((\d+))");
std::string text2 = "123";
std::string result2 = std::regex_replace(text2, group_pattern, "$2");  // Ошибка: группа $2 не существует
```

## Важные замечания:
1. Строка замены может содержать ссылки на группы ($1, $2, и т.д.)
2. Для экранирования $ в строке замены используйте $$
3. Флаги форматирования могут изменять поведение замены
4. Результат всегда возвращается как новая строка
5. Важно правильно экранировать специальные символы в шаблоне

## Производительность:
- Замена всех вхождений может быть ресурсоемкой
- Сложные шаблоны могут работать медленно
- Использование флагов может влиять на производительность
- Рекомендуется использовать более простые шаблоны, где это возможно 