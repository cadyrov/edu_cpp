# Как работает `string`?

## Ответ
`std::string` - это класс для работы со строками в C++. Он предоставляет удобный интерфейс для работы с текстовыми данными, автоматически управляя памятью и предоставляя множество полезных методов.

Основные характеристики:
1. Динамическое изменение размера
2. Автоматическое управление памятью
3. Поддержка итераторов и алгоритмов STL
4. Безопасный доступ к элементам
5. Поддержка различных кодировок

## Примеры кода

### Правильное использование

```cpp
// Создание строк
std::string s1 = "Hello";
std::string s2("World");
std::string s3(5, '*');  // "*****"

// Конкатенация
std::string result = s1 + " " + s2;  // "Hello World"
s1 += "!";  // "Hello!"

// Доступ к элементам
char first = s1[0];      // 'H'
char last = s1.back();   // '!'
char safe = s1.at(1);    // 'e' (с проверкой границ)

// Поиск
size_t pos = s1.find("lo");  // 3
size_t rpos = s1.rfind("l"); // 3 (поиск с конца)
bool contains = s1.find("World") != std::string::npos;

// Подстроки
std::string sub = s1.substr(0, 5);  // "Hello"
s1.replace(0, 5, "Hi");  // "Hi!"

// Размер и емкость
size_t len = s1.length();  // 3
size_t cap = s1.capacity();  // >= 3
s1.reserve(100);  // Увеличить емкость

// Итерация
for (char c : s1) {
    std::cout << c;
}

// Преобразования
int num = std::stoi("42");
double d = std::stod("3.14");
std::string str = std::to_string(42);
```

### Неправильное использование

```cpp
// Неправильно: доступ к несуществующему индексу
std::string s = "Hello";
char c = s[10];  // Неопределенное поведение

// Правильно: использование at() или проверка
char safe = s.at(10);  // Выбросит исключение
if (10 < s.length()) {
    char c = s[10];
}

// Неправильно: конкатенация в цикле
std::string result;
for (int i = 0; i < 1000; ++i) {
    result += "x";  // Неэффективно
}

// Правильно: предварительное выделение памяти
std::string result;
result.reserve(1000);
for (int i = 0; i < 1000; ++i) {
    result += "x";
}

// Неправильно: использование C-строк без проверки
const char* cstr = "Hello";
std::string s(cstr);  // Опасно, если cstr == nullptr

// Правильно: проверка указателя
if (cstr != nullptr) {
    std::string s(cstr);
}
```

## Рекомендации по использованию

1. Используйте `std::string` когда:
   - Работаете с текстовыми данными
   - Нужна динамическая строка
   - Требуется безопасная работа со строками
   - Нужны встроенные методы для работы со строками
   - Важна совместимость со STL

2. Не используйте `std::string` когда:
   - Работаете с фиксированными строками
   - Нужна максимальная производительность
   - Требуется работа с бинарными данными
   - Нужна поддержка специфических кодировок
   - Важна минимальная память

## Особенности реализации

```cpp
// Пример реализации string
class String {
    char* data;
    size_t size;
    size_t capacity;
    
public:
    // Конструкторы
    String() : data(nullptr), size(0), capacity(0) {}
    
    String(const char* str) {
        size = strlen(str);
        capacity = size + 1;
        data = new char[capacity];
        strcpy(data, str);
    }
    
    // Деструктор
    ~String() {
        delete[] data;
    }
    
    // Копирование
    String(const String& other) {
        size = other.size;
        capacity = other.capacity;
        data = new char[capacity];
        strcpy(data, other.data);
    }
    
    // Перемещение
    String(String&& other) noexcept {
        data = other.data;
        size = other.size;
        capacity = other.capacity;
        other.data = nullptr;
        other.size = 0;
        other.capacity = 0;
    }
    
    // Операторы
    String& operator=(const String& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            capacity = other.capacity;
            data = new char[capacity];
            strcpy(data, other.data);
        }
        return *this;
    }
    
    String& operator+=(const String& other) {
        size_t new_size = size + other.size;
        if (new_size >= capacity) {
            reserve(new_size + 1);
        }
        strcpy(data + size, other.data);
        size = new_size;
        return *this;
    }
    
    // Методы
    void reserve(size_t new_capacity) {
        if (new_capacity > capacity) {
            char* new_data = new char[new_capacity];
            if (data != nullptr) {
                strcpy(new_data, data);
                delete[] data;
            }
            data = new_data;
            capacity = new_capacity;
        }
    }
    
    size_t length() const { return size; }
    bool empty() const { return size == 0; }
    char& operator[](size_t pos) { return data[pos]; }
    const char& operator[](size_t pos) const { return data[pos]; }
};
``` 