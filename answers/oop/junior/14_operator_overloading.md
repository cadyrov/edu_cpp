# Перегрузка операторов

## Вопрос
Как работает перегрузка операторов?

## Ответ
Перегрузка операторов в C++ позволяет определить собственное поведение операторов для пользовательских типов данных. Это позволяет использовать объекты классов с операторами так же естественно, как и встроенные типы.

Основные характеристики:
- Можно перегружать большинство операторов
- Нельзя создавать новые операторы
- Нельзя менять приоритет операторов
- Нельзя менять количество операндов
- Операторы могут быть перегружены как методы класса или как глобальные функции
- Некоторые операторы должны быть перегружены как методы класса

## Примеры кода

### Правильный пример:
```cpp
class Complex {
private:
    double real;
    double imag;
    
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // Перегрузка оператора сложения как метода класса
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }
    
    // Перегрузка оператора вычитания
    Complex operator-(const Complex& other) const {
        return Complex(real - other.real, imag - other.imag);
    }
    
    // Перегрузка оператора присваивания
    Complex& operator=(const Complex& other) {
        if (this != &other) {  // Проверка на самоприсваивание
            real = other.real;
            imag = other.imag;
        }
        return *this;
    }
    
    // Перегрузка оператора вывода в поток
    friend std::ostream& operator<<(std::ostream& os, const Complex& c) {
        os << c.real << " + " << c.imag << "i";
        return os;
    }
    
    // Перегрузка оператора сравнения
    bool operator==(const Complex& other) const {
        return real == other.real && imag == other.imag;
    }
};

// Пример использования
void example() {
    Complex c1(3, 4);
    Complex c2(1, 2);
    
    // Использование перегруженных операторов
    Complex c3 = c1 + c2;  // Сложение
    Complex c4 = c1 - c2;  // Вычитание
    
    std::cout << "c1: " << c1 << std::endl;
    std::cout << "c2: " << c2 << std::endl;
    std::cout << "c1 + c2: " << c3 << std::endl;
    std::cout << "c1 - c2: " << c4 << std::endl;
    
    if (c1 == c2) {
        std::cout << "c1 equals c2" << std::endl;
    }
}
```

### Неправильный пример:
```cpp
class BadComplex {
private:
    double real;
    double imag;
    
public:
    BadComplex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // Неправильная перегрузка оператора сложения
    void operator+(const BadComplex& other) {  // Возвращает void вместо BadComplex
        real += other.real;
        imag += other.imag;
    }
    
    // Неправильная перегрузка оператора присваивания
    BadComplex operator=(const BadComplex& other) {  // Возвращает копию вместо ссылки
        real = other.real;
        imag = other.imag;
        return *this;
    }
    
    // Неправильная перегрузка оператора вывода
    std::ostream& operator<<(std::ostream& os) {  // Неправильная сигнатура
        os << real << " + " << imag << "i";
        return os;
    }
    
    // Попытка изменить приоритет оператора
    BadComplex operator*(const BadComplex& other) const {
        return BadComplex(real * other.real, imag * other.imag);
    }
    
    // Попытка создать новый оператор
    BadComplex operator**(const BadComplex& other) const {  // Ошибка: нельзя создать новый оператор
        return BadComplex(std::pow(real, other.real), std::pow(imag, other.imag));
    }
};

// Пример проблемного использования
void badExample() {
    BadComplex c1(3, 4);
    BadComplex c2(1, 2);
    
    // Неправильное использование операторов
    c1 + c2;  // Результат теряется
    BadComplex c3 = c1;  // Неправильное присваивание
    
    // c1 << std::cout;  // Неправильный синтаксис вывода
    
    // c1 ** c2;  // Ошибка компиляции: неизвестный оператор
}
```

## Комментарии
- В правильном примере:
  - Корректная перегрузка операторов
  - Правильные возвращаемые значения
  - Проверка на самоприсваивание
  - Использование friend для операторов ввода/вывода
  - Соблюдение семантики операторов

- В неправильном примере:
  - Неправильные возвращаемые значения
  - Отсутствие проверки на самоприсваивание
  - Неправильные сигнатуры операторов
  - Попытка изменить приоритет операторов
  - Попытка создать новые операторы 