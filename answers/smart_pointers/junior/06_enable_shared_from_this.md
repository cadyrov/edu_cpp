# Как работает `enable_shared_from_this`?

## Ответ
`enable_shared_from_this` - это шаблонный класс, который позволяет объекту безопасно создавать `shared_ptr` на самого себя. Это полезно, когда объект должен передать `shared_ptr` на себя в другие функции или сохранить его в своих полях. Без `enable_shared_from_this` создание `shared_ptr` из `this` может привести к множественному удалению объекта.

## Примеры кода

### Правильное использование:
```cpp
class MyClass : public std::enable_shared_from_this<MyClass> {
public:
    std::shared_ptr<MyClass> getShared() {
        return shared_from_this();
    }

    void process() {
        // Безопасное создание shared_ptr на себя
        auto self = shared_from_this();
        // Использование self
    }
};

// Использование
auto obj = std::make_shared<MyClass>();
auto ptr = obj->getShared(); // Безопасно
```

### Неправильное использование:
```cpp
class WrongClass {
public:
    std::shared_ptr<WrongClass> getShared() {
        return std::shared_ptr<WrongClass>(this); // Ошибка: множественное удаление
    }
};

// Ошибка: создание shared_ptr из this без enable_shared_from_this
class BadClass {
public:
    void badMethod() {
        auto ptr = std::shared_ptr<BadClass>(this); // Ошибка: множественное удаление
    }
};

// Ошибка: вызов shared_from_this до создания shared_ptr
class MyClass : public std::enable_shared_from_this<MyClass> {
public:
    void wrongMethod() {
        auto ptr = shared_from_this(); // Ошибка: объект еще не управляется shared_ptr
    }
};
```

## Комментарии
- Наследуйтесь от `enable_shared_from_this` если класс должен создавать `shared_ptr` на себя
- Объект должен уже быть управляем `shared_ptr` перед вызовом `shared_from_this`
- Не создавайте `shared_ptr` напрямую из `this` без `enable_shared_from_this`
- `enable_shared_from_this` добавляет небольшой оверхед к размеру объекта
- Используйте `shared_from_this()` вместо создания нового `shared_ptr` из `this` 