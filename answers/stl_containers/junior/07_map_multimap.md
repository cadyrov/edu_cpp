# Как работает `map` и `multimap`?

## Ответ
`std::map` и `std::multimap` - это ассоциативные контейнеры, которые хранят пары ключ-значение в отсортированном порядке по ключу. Основное различие между ними:
- `map` хранит только уникальные ключи
- `multimap` может хранить дубликаты ключей

Основные характеристики:
1. Элементы всегда отсортированы по ключу
2. Поиск, вставка и удаление - O(log n)
3. Элементы хранятся в виде сбалансированного дерева
4. Итераторы остаются валидными после вставки/удаления
5. Ключи должны поддерживать операцию сравнения

## Примеры кода

### Правильное использование

```cpp
// Создание и инициализация map
std::map<std::string, int> m = {
    {"apple", 1},
    {"banana", 2},
    {"orange", 3}
};

// Вставка элементов
m["grape"] = 4;  // Использование оператора []
m.insert({"pear", 5});  // Использование insert

// Создание и инициализация multimap
std::multimap<std::string, int> mm = {
    {"apple", 1},
    {"apple", 2},  // Допустим дубликат ключа
    {"banana", 3}
};

// Вставка элементов в multimap
mm.insert({"apple", 4});  // Добавляет новый элемент с тем же ключом

// Поиск элементов
auto it = m.find("apple");
if (it != m.end()) {
    std::cout << "Значение для apple: " << it->second << std::endl;
}

// Удаление элементов
m.erase("banana");  // Удаление по ключу
mm.erase("apple");  // Удаление всех элементов с ключом "apple"

// Проверка наличия ключа
if (m.count("orange") > 0) {
    std::cout << "Ключ orange найден" << std::endl;
}
```

### Неправильное использование

```cpp
// Неправильно: попытка изменить ключ
std::map<std::string, int> m = {{"apple", 1}, {"banana", 2}};
auto it = m.find("apple");
it->first = "pear";  // Ошибка! Ключи map нельзя изменять

// Правильно: удаление и вставка новой пары
std::map<std::string, int> m = {{"apple", 1}, {"banana", 2}};
auto it = m.find("apple");
int value = it->second;
m.erase(it);
m.insert({"pear", value});

// Неправильно: использование map для частых вставок/удалений
std::map<int, int> m;
for (int i = 0; i < 1000; ++i) {
    m[i] = i;     // O(log n) операция
    m.erase(i);   // O(log n) операция
}

// Правильно: использование vector для частых вставок/удалений
std::vector<std::pair<int, int>> v;
for (int i = 0; i < 1000; ++i) {
    v.push_back({i, i});  // O(1) операция
    v.pop_back();         // O(1) операция
}
```

## Рекомендации по использованию

1. Используйте `std::map` когда:
   - Нужны уникальные ключи
   - Требуется быстрый поиск по ключу
   - Элементы должны быть отсортированы по ключу
   - Нужна гарантия уникальности ключей

2. Используйте `std::multimap` когда:
   - Допустимы дубликаты ключей
   - Требуется быстрый поиск по ключу
   - Элементы должны быть отсортированы по ключу
   - Нужно хранить несколько значений для одного ключа

3. Не используйте `map`/`multimap` когда:
   - Нужны частые вставки/удаления
   - Требуется произвольный доступ к элементам
   - Важна компактность в памяти
   - Ключи не поддерживают сравнение

## Особенности реализации

```cpp
// Пример структуры узла красно-черного дерева для map
template<typename K, typename V>
struct Node {
    std::pair<K, V> data;
    Node* left;
    Node* right;
    Node* parent;
    bool is_red;
};

// Пример вставки элемента в map
template<typename K, typename V>
Node<K,V>* insert(Node<K,V>* root, const K& key, const V& value) {
    if (root == nullptr) {
        return new Node<K,V>{{key, value}, nullptr, nullptr, nullptr, false};
    }
    
    if (key < root->data.first) {
        root->left = insert(root->left, key, value);
        root->left->parent = root;
    } else if (key > root->data.first) {
        root->right = insert(root->right, key, value);
        root->right->parent = root;
    }
    
    // Балансировка дерева
    // ...
    
    return root;
}
``` 