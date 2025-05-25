# Наследование

## Вопрос
Как работает наследование?

## Ответ
Наследование - это механизм ООП, который позволяет создавать новые классы на основе существующих. Новый класс (производный) наследует свойства и методы базового класса.

Основные характеристики наследования:
- Производный класс получает все члены базового класса
- Можно добавлять новые члены в производный класс
- Можно переопределять методы базового класса
- Поддерживает иерархию классов
- Позволяет использовать полиморфизм

## Примеры кода

### Правильный пример:
```cpp
// Базовый класс
class Animal {
protected:
    std::string name;
    int age;

public:
    Animal(const std::string& n, int a) : name(n), age(a) {}
    
    virtual void makeSound() = 0;  // Чисто виртуальная функция
    
    virtual ~Animal() = default;  // Виртуальный деструктор
};

// Производный класс
class Dog : public Animal {
private:
    std::string breed;

public:
    Dog(const std::string& n, int a, const std::string& b)
        : Animal(n, a), breed(b) {}
    
    void makeSound() override {
        std::cout << "Woof!" << std::endl;
    }
    
    void fetch() {
        std::cout << name << " is fetching the ball" << std::endl;
    }
};

// Многоуровневое наследование
class WorkingDog : public Dog {
private:
    std::string job;

public:
    WorkingDog(const std::string& n, int a, const std::string& b, 
               const std::string& j)
        : Dog(n, a, b), job(j) {}
    
    void work() {
        std::cout << name << " is working as a " << job << std::endl;
    }
};
```

### Неправильный пример:
```cpp
// Неправильное использование наследования
class BadAnimal {
public:
    std::string name;
    int age;
    
    void makeSound() {  // Не виртуальная функция
        std::cout << "Some sound" << std::endl;
    }
};

// Наследование без виртуального деструктора
class BadDog : public BadAnimal {
public:
    std::string breed;
    
    void makeSound() {  // Нет override
        std::cout << "Woof!" << std::endl;
    }
};

// Наследование от конкретного класса
class BadPet : private BadAnimal {
public:
    // Нарушение инкапсуляции
    using BadAnimal::name;  // Публичный доступ к приватному члену
};
```

## Комментарии
- В правильном примере:
  - Использование виртуальных функций
  - Правильное наследование
  - Виртуальный деструктор
  - Корректная инициализация базового класса
  - Использование override
  - Многоуровневое наследование

- В неправильном примере:
  - Отсутствие виртуальных функций
  - Нет виртуального деструктора
  - Нарушение инкапсуляции
  - Неправильное использование using
  - Отсутствие override 