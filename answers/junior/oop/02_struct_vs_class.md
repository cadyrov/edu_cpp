# Разница между struct и class

## Вопрос
В чем разница между `struct` и `class`?

## Ответ
В C++ `struct` и `class` практически идентичны, но имеют следующие ключевые различия:

1. **Модификатор доступа по умолчанию**:
   - В `struct` все члены по умолчанию публичные (`public`)
   - В `class` все члены по умолчанию приватные (`private`)

2. **Наследование по умолчанию**:
   - В `struct` наследование по умолчанию публичное (`public`)
   - В `class` наследование по умолчанию приватное (`private`)

3. **Семантическое использование**:
   - `struct` обычно используется для простых структур данных
   - `class` обычно используется для сложных объектов с инкапсуляцией

## Примеры кода

### Правильный пример:
```cpp
// Использование struct для простой структуры данных
struct Point {
    double x;
    double y;
    
    // Можно добавлять методы, но обычно struct содержит только данные
    double distance(const Point& other) const {
        return std::sqrt(std::pow(x - other.x, 2) + std::pow(y - other.y, 2));
    }
};

// Использование class для сложного объекта
class Rectangle {
private:
    Point topLeft;
    Point bottomRight;
    
public:
    Rectangle(const Point& tl, const Point& br) 
        : topLeft(tl), bottomRight(br) {}
    
    double area() const {
        return std::abs((bottomRight.x - topLeft.x) * 
                       (bottomRight.y - topLeft.y));
    }
    
    void move(double dx, double dy) {
        topLeft.x += dx;
        topLeft.y += dy;
        bottomRight.x += dx;
        bottomRight.y += dy;
    }
};
```

### Неправильный пример:
```cpp
// Неправильное использование struct
struct BadRectangle {
    // Все поля публичные - нарушение инкапсуляции
    double width;
    double height;
    std::string color;
    
    // Сложная логика в struct - лучше использовать class
    void calculateArea() {
        // ...
    }
    
    void validateDimensions() {
        // ...
    }
};

// Неправильное использование class
class BadPoint {
private:
    // Излишняя инкапсуляция для простой структуры данных
    double x;
    double y;
    
public:
    BadPoint(double x, double y) : x(x), y(y) {}
    
    double getX() const { return x; }
    double getY() const { return y; }
    void setX(double newX) { x = newX; }
    void setY(double newY) { y = newY; }
};
```

## Комментарии
- В правильном примере:
  - `struct` используется для простых структур данных
  - `class` используется для сложных объектов с инкапсуляцией
  - Соблюдены принципы ООП
  - Правильное использование модификаторов доступа

- В неправильном примере:
  - `struct` содержит сложную логику
  - `class` используется для простой структуры данных
  - Излишняя инкапсуляция
  - Нарушение принципов ООП 