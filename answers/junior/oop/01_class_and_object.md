# Класс и объект

## Вопрос
Что такое класс и объект?

## Ответ
Класс - это пользовательский тип данных, который определяет структуру и поведение объектов. Он служит шаблоном для создания объектов. Объект - это экземпляр класса, который содержит конкретные данные и может выполнять определенные действия.

Основные характеристики:
- Класс определяет свойства (поля) и поведение (методы)
- Объект имеет свое собственное состояние (значения полей)
- Класс существует только в коде
- Объект существует во время выполнения программы

## Примеры кода

### Правильный пример:
```cpp
// Определение класса
class Student {
private:
    std::string name;
    int age;
    std::vector<int> grades;

public:
    // Конструктор
    Student(const std::string& n, int a) : name(n), age(a) {}
    
    // Методы
    void addGrade(int grade) {
        if (grade >= 0 && grade <= 100) {
            grades.push_back(grade);
        }
    }
    
    double getAverageGrade() const {
        if (grades.empty()) return 0.0;
        return std::accumulate(grades.begin(), grades.end(), 0.0) / grades.size();
    }
    
    std::string getName() const { return name; }
    int getAge() const { return age; }
};

// Создание и использование объектов
int main() {
    // Создание объектов класса Student
    Student john("John", 20);
    Student alice("Alice", 19);
    
    // Использование объектов
    john.addGrade(85);
    john.addGrade(90);
    alice.addGrade(95);
    
    std::cout << "John's average grade: " << john.getAverageGrade() << std::endl;
    std::cout << "Alice's average grade: " << alice.getAverageGrade() << std::endl;
    
    return 0;
}
```

### Неправильный пример:
```cpp
// Неправильное определение класса
class BadStudent {
public:
    // Все поля публичные - нарушение инкапсуляции
    std::string name;
    int age;
    std::vector<int> grades;
    
    // Отсутствие конструктора
    // Отсутствие проверок в методах
    void addGrade(int grade) {
        grades.push_back(grade);  // Нет проверки на корректность оценки
    }
    
    double getAverageGrade() {
        return std::accumulate(grades.begin(), grades.end(), 0.0) / grades.size();
        // Нет проверки на пустой вектор
    }
};

// Неправильное использование объектов
int main() {
    BadStudent student;  // Создание объекта без инициализации
    student.age = -5;    // Некорректное значение
    student.addGrade(-10);  // Некорректная оценка
    return 0;
}
```

## Комментарии
- В правильном примере:
  - Четкое разделение интерфейса и реализации
  - Использование конструктора для инициализации
  - Защита данных через private
  - Проверки в методах
  - Константные методы где это уместно

- В неправильном примере:
  - Отсутствие инкапсуляции
  - Нет конструктора
  - Отсутствие проверок
  - Прямой доступ к полям
  - Возможность создания некорректного состояния объекта 