# Работа со строками в C++

## Сравнение строк
- Лексикографическое сравнение (по ASCII)
- Операции: ==, !=, >, <, <=, >=
- Правила сравнения:
  1. Сначала сравниваются длины
  2. Затем посимвольно до первого различия
  3. Заглавные буквы идут перед строчными

### Примеры сравнения
```cpp
"professor"s < "programming"s  // 'f' < 'g'
"Python"s < "c++"s            // Заглавные перед строчными
"Develop"s < "Developing"s    // Короткая строка меньше длинной
```

## Поиск в строках

### Поиск символа
```cpp
std::string str = "Hello World!";
std::size_t pos = str.find('o');  // Находит первое вхождение
std::size_t pos2 = str.find('o', pos + 1);  // Находит следующее вхождение
```

### Поиск подстроки
```cpp
std::string str = "Hello World!";
std::size_t pos = str.find("World");  // Находит подстроку
```

### Специальные методы поиска
```cpp
// Поиск любого символа из набора
str.find_first_of(",.?!");

// Поиск справа налево
str.rfind('o');

// Поиск символа, не входящего в набор
str.find_first_not_of("abc");
```

## Работа с подстроками

### Метод substr
```cpp
std::string str = "Hello World!";
// substr(начальная_позиция, длина)
std::string sub = str.substr(6, 5);  // "World"

// Получение подстроки до конца
std::string end = str.substr(6);  // "World!"
```

### Пример извлечения слова
```cpp
std::string str = "Hello World!";
std::size_t first_space = str.find(' ');
std::size_t next_space = str.find(' ', first_space + 1);
std::size_t word_length = next_space - first_space - 1;
std::string word = str.substr(first_space + 1, word_length);
```

## Важные моменты
1. Используйте суффикс `s` для строковых литералов
2. `std::string::npos` - специальное значение "не найдено"
3. Индексация начинается с 0
4. При поиске можно указать начальную позицию
5. Метод `substr` не изменяет исходную строку 