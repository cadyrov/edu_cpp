# Как работает `std::filesystem`?

## Ответ
`std::filesystem` - это библиотека, введенная в C++17, которая предоставляет набор классов и функций для работы с файловой системой. Она обеспечивает кроссплатформенный доступ к файлам и директориям.

### Основные возможности:

1. **Работа с путями**
   - Создание и манипуляция путями
   - Нормализация путей
   - Работа с относительными и абсолютными путями

2. **Операции с файлами**
   - Создание, удаление, копирование, перемещение
   - Проверка существования
   - Получение информации о файлах

3. **Работа с директориями**
   - Создание и удаление директорий
   - Обход содержимого
   - Рекурсивные операции

## Примеры кода

### Правильное использование:

```cpp
// Работа с путями
std::filesystem::path p("dir/subdir/file.txt");
std::cout << "Filename: " << p.filename() << '\n';
std::cout << "Extension: " << p.extension() << '\n';
std::cout << "Parent: " << p.parent_path() << '\n';

// Создание директории
std::filesystem::create_directory("new_dir");
std::filesystem::create_directories("dir/subdir");  // Создает все промежуточные директории

// Проверка существования
if (std::filesystem::exists("file.txt")) {
    std::cout << "File exists\n";
}

// Получение информации о файле
auto fileSize = std::filesystem::file_size("file.txt");
auto lastWriteTime = std::filesystem::last_write_time("file.txt");

// Копирование файла
std::filesystem::copy_file("source.txt", "destination.txt");

// Обход директории
for (const auto& entry : std::filesystem::directory_iterator("dir")) {
    if (entry.is_regular_file()) {
        std::cout << "File: " << entry.path() << '\n';
    } else if (entry.is_directory()) {
        std::cout << "Directory: " << entry.path() << '\n';
    }
}

// Рекурсивный обход
for (const auto& entry : std::filesystem::recursive_directory_iterator("dir")) {
    std::cout << entry.path() << '\n';
}
```

### Неправильное использование:

```cpp
// Неправильное использование путей
std::filesystem::path p;
p = "file.txt";  // Опасность: относительный путь
// Лучше использовать:
p = std::filesystem::absolute("file.txt");

// Неправильное копирование файла
std::filesystem::copy_file("source.txt", "destination.txt");  // Ошибка: не проверяется существование
// Правильно:
if (std::filesystem::exists("source.txt")) {
    std::filesystem::copy_file("source.txt", "destination.txt");
}

// Неправильное удаление
std::filesystem::remove("file.txt");  // Ошибка: не проверяется существование
// Правильно:
if (std::filesystem::exists("file.txt")) {
    std::filesystem::remove("file.txt");
}

// Неправильное использование с временными файлами
std::filesystem::path temp = std::filesystem::temp_directory_path() / "temp.txt";
// Опасность: файл может остаться после завершения программы
// Лучше использовать RAII:
class TempFile {
    std::filesystem::path path;
public:
    TempFile() : path(std::filesystem::temp_directory_path() / "temp.txt") {}
    ~TempFile() { std::filesystem::remove(path); }
};

// Неправильное использование с правами доступа
std::filesystem::permissions("file.txt", std::filesystem::perms::owner_all);  // Опасность: может не работать на Windows
// Лучше использовать:
std::filesystem::permissions("file.txt", std::filesystem::perms::owner_all, std::filesystem::perm_options::add);
```

## Важные замечания
1. Всегда проверяйте существование файлов перед операциями
2. Используйте абсолютные пути для надежности
3. Обрабатывайте исключения при работе с файловой системой
4. Используйте RAII для временных файлов
5. Учитывайте кроссплатформенные особенности при работе с правами доступа 