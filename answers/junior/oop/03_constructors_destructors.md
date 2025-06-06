# Конструкторы и деструкторы

## Вопрос
Что такое конструктор и деструктор?

## Ответ
Конструктор - это специальный метод класса, который вызывается при создании объекта. Он инициализирует объект и выделяет необходимые ресурсы. Деструктор - это метод, который вызывается при уничтожении объекта и освобождает выделенные ресурсы.

Основные характеристики:
- Конструктор имеет то же имя, что и класс
- Деструктор имеет имя класса с префиксом `~`
- Конструктор может быть перегружен (несколько версий)
- Деструктор всегда один и не может быть перегружен
- Деструктор не принимает параметров
- Деструктор вызывается автоматически при уничтожении объекта

## Примеры кода

### Правильный пример:
```cpp
class Resource {
private:
    int* data;
    size_t size;

public:
    // Конструктор по умолчанию
    Resource() : data(nullptr), size(0) {
        std::cout << "Default constructor called\n";
    }

    // Параметризованный конструктор
    Resource(size_t s) : size(s) {
        data = new int[size];
        std::cout << "Parameterized constructor called\n";
    }

    // Копирующий конструктор
    Resource(const Resource& other) : size(other.size) {
        data = new int[size];
        std::copy(other.data, other.data + size, data);
        std::cout << "Copy constructor called\n";
    }

    // Деструктор
    ~Resource() {
        delete[] data;
        std::cout << "Destructor called\n";
    }
};

// Пример использования
void example() {
    Resource r1;           // Вызов конструктора по умолчанию
    Resource r2(10);       // Вызов параметризованного конструктора
    Resource r3 = r2;      // Вызов копирующего конструктора
} // Все деструкторы вызываются автоматически
```

### Неправильный пример:
```cpp
class BadResource {
public:
    int* data;
    size_t size;

    // Отсутствие конструктора по умолчанию
    // Неправильная инициализация в конструкторе
    BadResource(size_t s) {
        size = s;  // Неправильная инициализация
        data = new int[size];  // Нет проверки на nullptr
    }

    // Отсутствие копирующего конструктора
    // Неправильный деструктор
    ~BadResource() {
        delete data;  // Использование delete вместо delete[]
        // Нет обнуления указателя
    }
};

// Пример проблемного использования
void badExample() {
    BadResource r1(10);
    BadResource r2 = r1;  // Проблема: двойное освобождение памяти
    // Нет обработки исключений
}
```

## Комментарии
- В правильном примере:
  - Все необходимые конструкторы реализованы
  - Правильная инициализация членов класса
  - Корректная работа с ресурсами
  - Использование списка инициализации
  - Правильное освобождение ресурсов в деструкторе

- В неправильном примере:
  - Отсутствие важных конструкторов
  - Неправильная инициализация
  - Проблемы с управлением памятью
  - Отсутствие проверок
  - Неправильное освобождение ресурсов 