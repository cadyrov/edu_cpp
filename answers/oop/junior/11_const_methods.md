# Const методы

## Вопрос
Что такое `const` методы?

## Ответ
`const` методы - это методы класса, которые не изменяют состояние объекта. Они гарантируют, что вызов метода не изменит данные объекта. Это важный механизм для обеспечения инкапсуляции и безопасности данных.

Основные характеристики:
- Не могут изменять нестатические члены класса
- Не могут вызывать не-const методы
- Могут быть вызваны для const объектов
- Могут быть перегружены с не-const версиями
- Помогают компилятору оптимизировать код
- Делают код более безопасным и предсказуемым

## Примеры кода

### Правильный пример:
```cpp
class Rectangle {
private:
    double width;
    double height;
    mutable int accessCount;  // Может быть изменен даже в const методах

public:
    Rectangle(double w, double h) : width(w), height(h), accessCount(0) {}
    
    // Const методы
    double getWidth() const {
        accessCount++;  // Можно изменить mutable член
        return width;
    }
    
    double getHeight() const {
        accessCount++;
        return height;
    }
    
    double getArea() const {
        accessCount++;
        return width * height;
    }
    
    int getAccessCount() const {
        return accessCount;
    }
    
    // Не-const методы
    void setWidth(double w) {
        width = w;
    }
    
    void setHeight(double h) {
        height = h;
    }
};

// Пример использования
void example() {
    const Rectangle rect(5, 10);  // Const объект
    
    // Вызов const методов
    std::cout << "Width: " << rect.getWidth() << std::endl;
    std::cout << "Height: " << rect.getHeight() << std::endl;
    std::cout << "Area: " << rect.getArea() << std::endl;
    
    // rect.setWidth(6);  // Ошибка: нельзя вызвать не-const метод для const объекта
    
    Rectangle nonConstRect(3, 4);  // Не-const объект
    nonConstRect.setWidth(5);  // Можно вызвать не-const метод
}
```

### Неправильный пример:
```cpp
class BadRectangle {
private:
    double width;
    double height;
    int accessCount;

public:
    BadRectangle(double w, double h) : width(w), height(h), accessCount(0) {}
    
    // Неправильное использование const
    double getWidth() const {
        width = 0;  // Ошибка: попытка изменить член в const методе
        return width;
    }
    
    // Отсутствие const где нужно
    double getHeight() {
        return height;  // Метод должен быть const
    }
    
    // Неправильное использование mutable
    mutable double area;  // Не должно быть mutable
    
    double getArea() const {
        area = width * height;  // Неправильное использование mutable
        return area;
    }
    
    // Неправильная перегрузка
    void setDimensions(double w, double h) const {
        width = w;  // Ошибка: попытка изменить члены в const методе
        height = h;
    }
};

// Пример проблемного использования
void badExample() {
    const BadRectangle rect(5, 10);
    
    // rect.getWidth();  // Ошибка компиляции
    
    double height = rect.getHeight();  // Небезопасно: метод не const
    
    double area = rect.getArea();  // Неправильное использование mutable
    
    // rect.setDimensions(6, 8);  // Ошибка компиляции
}
```

## Комментарии
- В правильном примере:
  - Корректное использование const методов
  - Правильное применение mutable
  - Четкое разделение const и не-const методов
  - Безопасное использование const объектов
  - Эффективное использование const

- В неправильном примере:
  - Попытка изменения членов в const методах
  - Отсутствие const где нужно
  - Неправильное использование mutable
  - Нарушение принципов const корректности
  - Небезопасное использование методов 