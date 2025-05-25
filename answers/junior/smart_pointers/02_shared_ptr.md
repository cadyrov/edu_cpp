# Как работает `shared_ptr`?

## Ответ
`shared_ptr` - это умный указатель, который реализует подсчет ссылок (reference counting). Он позволяет нескольким указателям владеть одним и тем же объектом. Объект автоматически удаляется, когда последний `shared_ptr`, указывающий на него, уничтожается или переназначается.

## Примеры кода

### Правильное использование:
```cpp
// Создание shared_ptr
std::shared_ptr<int> ptr1 = std::make_shared<int>(42);

// Копирование shared_ptr
std::shared_ptr<int> ptr2 = ptr1; // Счетчик ссылок увеличивается до 2

// Использование в функции
void process(std::shared_ptr<int> value) {
    // Работа с value
} // Счетчик ссылок уменьшается при выходе из функции

// Создание shared_ptr из weak_ptr
std::weak_ptr<int> weak = ptr1;
std::shared_ptr<int> ptr3 = weak.lock(); // Безопасное получение shared_ptr
```

### Неправильное использование:
```cpp
// Ошибка: циклические ссылки
struct Node {
    std::shared_ptr<Node> next;
};

std::shared_ptr<Node> node1 = std::make_shared<Node>();
std::shared_ptr<Node> node2 = std::make_shared<Node>();
node1->next = node2;
node2->next = node1; // Создаем циклическую ссылку

// Ошибка: не используйте raw pointer для создания shared_ptr
int* raw_ptr = new int(42);
std::shared_ptr<int> ptr(raw_ptr); // Потенциально опасно
```

## Комментарии
- Всегда используйте `std::make_shared` вместо прямого создания через `new`
- `shared_ptr` подходит для случаев, когда объект должен принадлежать нескольким владельцам
- Будьте осторожны с циклическими ссылками - используйте `weak_ptr` для их разрыва
- `shared_ptr` имеет накладные расходы на подсчет ссылок
- Не создавайте `shared_ptr` из raw pointer, если этот указатель уже управляется другим `shared_ptr` 