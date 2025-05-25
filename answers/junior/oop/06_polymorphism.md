# Полиморфизм

## Вопрос
Как работает полиморфизм?

## Ответ
Полиморфизм - это способность объектов с одинаковым интерфейсом иметь различную реализацию. В C++ полиморфизм реализуется через виртуальные функции и может быть двух типов:

1. **Полиморфизм времени выполнения** (динамический):
   - Реализуется через виртуальные функции
   - Решение о том, какую версию функции вызвать, принимается во время выполнения
   - Требует использования указателей или ссылок на базовый класс

2. **Полиморфизм времени компиляции** (статический):
   - Реализуется через перегрузку функций и шаблоны
   - Решение о том, какую версию функции вызвать, принимается во время компиляции

## Примеры кода

### Правильный пример:
```cpp
// Базовый класс
class Animal {
protected:
    std::string name;

public:
    Animal(const std::string& n) : name(n) {}
    
    // Виртуальная функция для полиморфизма
    virtual void makeSound() const = 0;
    
    // Виртуальная функция для полиморфизма
    virtual void move() const = 0;
    
    virtual ~Animal() = default;
};

// Производные классы
class Dog : public Animal {
public:
    Dog(const std::string& n) : Animal(n) {}
    
    void makeSound() const override {
        std::cout << name << " says: Woof!" << std::endl;
    }
    
    void move() const override {
        std::cout << name << " is running on four legs" << std::endl;
    }
};

class Bird : public Animal {
public:
    Bird(const std::string& n) : Animal(n) {}
    
    void makeSound() const override {
        std::cout << name << " says: Tweet!" << std::endl;
    }
    
    void move() const override {
        std::cout << name << " is flying" << std::endl;
    }
};

// Пример использования полиморфизма
void example() {
    // Создаем массив указателей на базовый класс
    std::vector<Animal*> animals;
    animals.push_back(new Dog("Rex"));
    animals.push_back(new Bird("Tweety"));
    
    // Полиморфный вызов методов
    for (const auto& animal : animals) {
        animal->makeSound();
        animal->move();
    }
    
    // Освобождение памяти
    for (auto animal : animals) {
        delete animal;
    }
}
```

### Неправильный пример:
```cpp
// Неправильная реализация полиморфизма
class BadAnimal {
public:
    std::string name;
    
    // Отсутствие виртуальных функций
    void makeSound() {
        std::cout << "Some sound" << std::endl;
    }
    
    void move() {
        std::cout << "Moving" << std::endl;
    }
};

class BadDog : public BadAnimal {
public:
    // Переопределение без virtual
    void makeSound() {
        std::cout << "Woof!" << std::endl;
    }
    
    void move() {
        std::cout << "Running" << std::endl;
    }
};

// Пример проблемного использования
void badExample() {
    BadDog dog;
    BadAnimal* animal = &dog;
    
    // Вызов невиртуальных функций
    // Будет вызвана функция базового класса
    animal->makeSound();  // Выведет "Some sound" вместо "Woof!"
    animal->move();       // Выведет "Moving" вместо "Running"
}
```

## Комментарии
- В правильном примере:
  - Использование виртуальных функций
  - Правильная реализация полиморфизма
  - Работа с указателями на базовый класс
  - Корректное освобождение памяти
  - Чистый и понятный интерфейс

- В неправильном примере:
  - Отсутствие виртуальных функций
  - Неправильная работа полиморфизма
  - Проблемы с вызовом методов через указатель на базовый класс
  - Отсутствие виртуального деструктора
  - Нарушение принципов ООП 