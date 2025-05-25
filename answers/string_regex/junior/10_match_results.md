# Как работает match_results?

`std::match_results` - это класс, который хранит результаты поиска регулярного выражения. Он содержит все найденные совпадения, включая группы, и предоставляет методы для работы с ними.

## Основные характеристики:
- Хранит все найденные совпадения
- Поддерживает доступ к группам
- Предоставляет информацию о позициях совпадений
- Может работать с различными типами строк

## Примеры использования:

```cpp
// Правильное использование
std::regex pattern(R"((\d{3})-(\d{2})-(\d{4}))");
std::string text = "SSN: 123-45-6789";
std::smatch matches;

// Поиск совпадений
if (std::regex_search(text, matches, pattern)) {
    // Получение полного совпадения
    std::string full_match = matches[0].str();  // "123-45-6789"
    
    // Работа с группами
    std::string area = matches[1].str();    // "123"
    std::string group = matches[2].str();   // "45"
    std::string serial = matches[3].str();  // "6789"
    
    // Проверка количества групп
    size_t group_count = matches.size();  // 4 (0 + 3 группы)
    
    // Получение позиций совпадений
    size_t start_pos = matches[0].first - text.begin();
    size_t end_pos = matches[0].second - text.begin();
    
    // Проверка наличия группы
    if (matches[1].matched) {
        std::cout << "Area code found\n";
    }
}

// Использование с regex_match
std::string date = "25/12/2023";
std::regex date_pattern(R"((\d{2})/(\d{2})/(\d{4}))");
std::smatch date_matches;

if (std::regex_match(date, date_matches, date_pattern)) {
    std::cout << "Day: " << date_matches[1] << '\n';
    std::cout << "Month: " << date_matches[2] << '\n';
    std::cout << "Year: " << date_matches[3] << '\n';
}
```

## Как не стоит использовать:

```cpp
// Неправильное использование
std::regex pattern(R"((\d+))");
std::string text = "abc123def";
std::smatch matches;

// Ошибка: обращение к результатам без проверки
std::string result = matches[0].str();  // Неопределенное поведение

// Неправильная работа с группами
if (std::regex_search(text, matches, pattern)) {
    std::string invalid = matches[2].str();  // Ошибка: группа 2 не существует
}

// Небезопасное использование
std::string input = "Very long string...";
std::regex dangerous_pattern(".*");
std::smatch dangerous_matches;
std::regex_search(input, dangerous_matches, dangerous_pattern);
// Может быть очень медленным и потреблять много памяти
```

## Важные замечания:
1. Всегда проверяйте результат поиска перед использованием
2. Группа 0 содержит полное совпадение
3. Группы нумеруются с 1
4. Метод `matched` позволяет проверить наличие совпадения
5. Итераторы становятся недействительными при изменении исходной строки

## Производительность:
- Создание `match_results` не требует выделения памяти
- Доступ к группам эффективен
- Преобразование в строку может быть дорогой операцией
- Рекомендуется использовать `str()` только при необходимости 