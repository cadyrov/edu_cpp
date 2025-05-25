# Override и Final

## Вопрос
Как использовать `override` и `final`?

## Ответ
`override` и `final` - это спецификаторы, которые помогают контролировать наследование и переопределение виртуальных функций в C++.

`override`:
- Указывает, что функция переопределяет виртуальную функцию из базового класса
- Помогает обнаружить ошибки при переопределении (например, опечатки в имени функции)
- Делает код более понятным и поддерживаемым
- Требует наличия виртуальной функции в базовом классе

`final`:
- Запрещает дальнейшее переопределение виртуальной функции
- Запрещает наследование от класса
- Помогает оптимизировать код
- Делает намерения программиста явными

## Примеры кода

### Правильный пример:
```cpp
class Animal {
public:
    virtual void makeSound() const = 0;
    virtual void move() const = 0;
    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    // Правильное использование override
    void makeSound() const override {
        std::cout << "Woof!" << std::endl;
    }
    
    void move() const override {
        std::cout << "Running on four legs" << std::endl;
    }
};

// Класс, который нельзя наследовать
class GermanShepherd final : public Dog {
public:
    // Функция, которую нельзя переопределить
    void makeSound() const override final {
        std::cout << "Loud Woof!" << std::endl;
    }
    
    void move() const override {
        std::cout << "Running very fast" << std::endl;
    }
};

// Пример использования
void example() {
    GermanShepherd dog;
    dog.makeSound();
    dog.move();
    
    // Нельзя наследовать от GermanShepherd
    // class Puppy : public GermanShepherd {};  // Ошибка компиляции
}
```

### Неправильный пример:
```cpp
class BadAnimal {
public:
    virtual void makeSound() {
        std::cout << "Some sound" << std::endl;
    }
};

class BadDog : public BadAnimal {
public:
    // Отсутствие override
    void makeSound() {  // Опечатка в имени функции
        std::cout << "Woof!" << std::endl;
    }
    
    // Неправильное использование override
    void move() override {  // Ошибка: нет виртуальной функции move в базовом классе
        std::cout << "Running" << std::endl;
    }
};

class BadGermanShepherd : public BadDog {
public:
    // Попытка переопределить final функцию
    void makeSound() override {  // Ошибка: функция не помечена как final
        std::cout << "Loud Woof!" << std::endl;
    }
};

// Пример проблемного использования
void badExample() {
    BadDog dog;
    BadAnimal* animal = &dog;
    
    animal->makeSound();  // Вызовет функцию базового класса из-за опечатки
}
```

## Комментарии
- В правильном примере:
  - Корректное использование override
  - Правильное применение final
  - Явное указание намерений
  - Защита от ошибок
  - Чистый и понятный код

- В неправильном примере:
  - Отсутствие override
  - Опечатки в именах функций
  - Неправильное использование override
  - Отсутствие final где нужно
  - Нарушение принципов наследования 