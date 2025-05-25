# Виртуальные функции

## Вопрос
Что такое виртуальные функции?

## Ответ
Виртуальные функции - это функции-члены класса, которые могут быть переопределены в производных классах. Они позволяют реализовать полиморфизм времени выполнения.

Основные характеристики:
- Объявляются с ключевым словом `virtual`
- Могут быть переопределены в производных классах
- Вызов происходит через указатель или ссылку на базовый класс
- Решение о том, какую версию функции вызвать, принимается во время выполнения
- Чисто виртуальные функции (abstract) не имеют реализации в базовом классе

## Примеры кода

### Правильный пример:
```cpp
class Shape {
protected:
    double x, y;

public:
    Shape(double x, double y) : x(x), y(y) {}
    
    // Виртуальная функция
    virtual double area() const {
        return 0.0;
    }
    
    // Чисто виртуальная функция
    virtual void draw() const = 0;
    
    // Виртуальный деструктор
    virtual ~Shape() = default;
};

class Circle : public Shape {
private:
    double radius;

public:
    Circle(double x, double y, double r) 
        : Shape(x, y), radius(r) {}
    
    // Переопределение виртуальной функции
    double area() const override {
        return 3.14159 * radius * radius;
    }
    
    // Реализация чисто виртуальной функции
    void draw() const override {
        std::cout << "Drawing circle at (" << x << "," << y 
                  << ") with radius " << radius << std::endl;
    }
};

// Пример использования
void example() {
    Circle circle(0, 0, 5);
    Shape* shape = &circle;  // Указатель на базовый класс
    
    // Вызов виртуальной функции
    std::cout << "Area: " << shape->area() << std::endl;
    shape->draw();
}
```

### Неправильный пример:
```cpp
class BadShape {
public:
    double x, y;
    
    // Отсутствие виртуальных функций
    double area() {
        return 0.0;
    }
    
    void draw() {
        std::cout << "Drawing shape" << std::endl;
    }
};

class BadCircle : public BadShape {
public:
    double radius;
    
    // Переопределение без virtual
    double area() {
        return 3.14159 * radius * radius;
    }
    
    void draw() {
        std::cout << "Drawing circle" << std::endl;
    }
};

// Пример проблемного использования
void badExample() {
    BadCircle circle;
    BadShape* shape = &circle;
    
    // Вызов невиртуальной функции
    // Будет вызвана функция базового класса
    shape->draw();  // Выведет "Drawing shape" вместо "Drawing circle"
}
```

## Комментарии
- В правильном примере:
  - Использование виртуальных функций
  - Правильное переопределение с override
  - Виртуальный деструктор
  - Чисто виртуальные функции для абстрактного класса
  - Корректная работа полиморфизма

- В неправильном примере:
  - Отсутствие виртуальных функций
  - Нет override
  - Отсутствие виртуального деструктора
  - Неправильная работа полиморфизма
  - Проблемы с вызовом методов через указатель на базовый класс 