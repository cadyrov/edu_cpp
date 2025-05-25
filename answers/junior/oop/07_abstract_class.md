# Абстрактный класс

## Вопрос
Что такое абстрактный класс?

## Ответ
Абстрактный класс - это класс, который содержит хотя бы одну чисто виртуальную функцию (pure virtual function). Абстрактный класс не может быть инстанцирован напрямую, он служит как интерфейс или базовый класс для других классов.

Основные характеристики абстрактных классов:
- Содержит хотя бы одну чисто виртуальную функцию
- Нельзя создать объект абстрактного класса
- Производные классы должны реализовать все чисто виртуальные функции
- Может содержать обычные методы с реализацией
- Может содержать виртуальные методы с реализацией

## Примеры кода

### Правильный пример:
```cpp
// Абстрактный класс
class Shape {
protected:
    double x, y;

public:
    Shape(double x, double y) : x(x), y(y) {}
    
    // Чисто виртуальная функция
    virtual double area() const = 0;
    
    // Обычная виртуальная функция с реализацией
    virtual void move(double dx, double dy) {
        x += dx;
        y += dy;
    }
    
    // Обычный метод
    void printPosition() const {
        std::cout << "Position: (" << x << ", " << y << ")" << std::endl;
    }
    
    virtual ~Shape() = default;
};

// Конкретный класс, наследующий абстрактный
class Circle : public Shape {
private:
    double radius;

public:
    Circle(double x, double y, double r) 
        : Shape(x, y), radius(r) {}
    
    // Реализация чисто виртуальной функции
    double area() const override {
        return 3.14159 * radius * radius;
    }
};

// Еще один конкретный класс
class Rectangle : public Shape {
private:
    double width, height;

public:
    Rectangle(double x, double y, double w, double h)
        : Shape(x, y), width(w), height(h) {}
    
    // Реализация чисто виртуальной функции
    double area() const override {
        return width * height;
    }
};

// Пример использования
void example() {
    // Shape shape(0, 0);  // Ошибка: нельзя создать объект абстрактного класса
    
    Circle circle(0, 0, 5);
    Rectangle rect(1, 1, 4, 6);
    
    // Работа с объектами через указатель на базовый класс
    Shape* shapes[] = {&circle, &rect};
    
    for (Shape* shape : shapes) {
        shape->printPosition();
        std::cout << "Area: " << shape->area() << std::endl;
        shape->move(1, 1);
    }
}
```

### Неправильный пример:
```cpp
// Неправильная реализация абстрактного класса
class BadShape {
public:
    double x, y;
    
    // Отсутствие чисто виртуальных функций
    virtual double area() {
        return 0.0;  // Это не абстрактный класс
    }
    
    // Попытка создать абстрактный класс без чисто виртуальных функций
    virtual void draw() {
        std::cout << "Drawing shape" << std::endl;
    }
};

class BadCircle : public BadShape {
public:
    double radius;
    
    // Необязательная реализация виртуальных функций
    double area() override {
        return 3.14159 * radius * radius;
    }
    
    // Можно не реализовывать draw()
};

// Пример проблемного использования
void badExample() {
    BadShape shape;  // Можно создать объект, хотя не должно быть возможно
    BadCircle circle;
    
    BadShape* shapes[] = {&shape, &circle};
    
    for (BadShape* shape : shapes) {
        shape->draw();  // Вызовет базовую реализацию для BadShape
    }
}
```

## Комментарии
- В правильном примере:
  - Использование чисто виртуальных функций
  - Невозможность создания объекта абстрактного класса
  - Обязательная реализация чисто виртуальных функций в производных классах
  - Правильное использование полиморфизма
  - Чистый и понятный интерфейс

- В неправильном примере:
  - Отсутствие чисто виртуальных функций
  - Возможность создания объекта базового класса
  - Необязательная реализация виртуальных функций
  - Нарушение принципов абстрактных классов
  - Неправильное использование полиморфизма 