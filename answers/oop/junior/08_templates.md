# Шаблоны

## Вопрос
Как работают шаблоны?

## Ответ
Шаблоны в C++ - это механизм, позволяющий создавать обобщенные классы и функции, которые могут работать с различными типами данных. Шаблоны обеспечивают полиморфизм времени компиляции.

Основные характеристики шаблонов:
- Позволяют писать код, независимый от конкретных типов
- Компилятор генерирует код для каждого используемого типа
- Могут быть шаблоны классов и шаблоны функций
- Поддерживают специализацию шаблонов
- Могут иметь несколько параметров типа

## Примеры кода

### Правильный пример:
```cpp
// Шаблон класса
template<typename T>
class Stack {
private:
    std::vector<T> elements;

public:
    void push(const T& element) {
        elements.push_back(element);
    }
    
    T pop() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        T element = elements.back();
        elements.pop_back();
        return element;
    }
    
    bool empty() const {
        return elements.empty();
    }
    
    size_t size() const {
        return elements.size();
    }
};

// Шаблон функции
template<typename T>
T max(T a, T b) {
    return (a > b) ? a : b;
}

// Специализация шаблона для строк
template<>
class Stack<std::string> {
private:
    std::vector<std::string> elements;

public:
    void push(const std::string& element) {
        elements.push_back(element);
    }
    
    std::string pop() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        std::string element = elements.back();
        elements.pop_back();
        return element;
    }
    
    bool empty() const {
        return elements.empty();
    }
    
    size_t size() const {
        return elements.size();
    }
    
    // Дополнительный метод для строкового стека
    void printAll() const {
        for (const auto& element : elements) {
            std::cout << element << std::endl;
        }
    }
};

// Пример использования
void example() {
    // Использование шаблона класса
    Stack<int> intStack;
    intStack.push(1);
    intStack.push(2);
    std::cout << intStack.pop() << std::endl;
    
    // Использование специализации
    Stack<std::string> stringStack;
    stringStack.push("Hello");
    stringStack.push("World");
    stringStack.printAll();
    
    // Использование шаблона функции
    int maxInt = max(5, 10);
    double maxDouble = max(3.14, 2.71);
}
```

### Неправильный пример:
```cpp
// Неправильное использование шаблонов
class BadStack {
public:
    std::vector<void*> elements;  // Использование void* вместо шаблонов
    
    template<typename T>
    void push(T element) {
        // Небезопасное приведение типов
        elements.push_back(static_cast<void*>(&element));
    }
    
    template<typename T>
    T pop() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        // Небезопасное приведение типов
        T* element = static_cast<T*>(elements.back());
        elements.pop_back();
        return *element;
    }
};

// Неправильное использование шаблонов функций
template<typename T, typename U>
T badMax(T a, U b) {
    // Смешивание разных типов может привести к неожиданным результатам
    return (a > b) ? a : b;
}

// Пример проблемного использования
void badExample() {
    BadStack stack;
    int x = 5;
    stack.push(x);  // Небезопасно, x может быть уничтожен
    
    int y = stack.pop<int>();  // Небезопасно, может быть некорректное приведение типов
    
    // Проблемное использование шаблона функции
    double result = badMax(5, 3.14);  // Неявное приведение типов
}
```

## Комментарии
- В правильном примере:
  - Безопасное использование шаблонов
  - Правильная специализация
  - Типобезопасность
  - Чистый и понятный интерфейс
  - Эффективное использование шаблонов

- В неправильном примере:
  - Небезопасное приведение типов
  - Использование void* вместо шаблонов
  - Смешивание разных типов в шаблонах функций
  - Потенциальные утечки памяти
  - Нарушение принципов типобезопасности 