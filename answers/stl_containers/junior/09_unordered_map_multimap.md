# Как использовать `unordered_map` и `unordered_multimap`?

## Ответ
`std::unordered_map` и `std::unordered_multimap` - это ассоциативные контейнеры, которые хранят пары ключ-значение в хеш-таблице. Основное различие между ними:
- `unordered_map` хранит только уникальные ключи
- `unordered_multimap` может хранить дубликаты ключей

Основные характеристики:
1. Элементы не отсортированы
2. Поиск, вставка и удаление - O(1) в среднем
3. Элементы хранятся в хеш-таблице
4. Итераторы могут стать невалидными после вставки/удаления
5. Ключи должны поддерживать хеширование

## Примеры кода

### Правильное использование

```cpp
// Создание и инициализация unordered_map
std::unordered_map<std::string, int> um = {
    {"apple", 1},
    {"banana", 2},
    {"orange", 3}
};

// Вставка элементов
um["grape"] = 4;  // Использование оператора []
um.insert({"pear", 5});  // Использование insert

// Создание и инициализация unordered_multimap
std::unordered_multimap<std::string, int> umm = {
    {"apple", 1},
    {"apple", 2},  // Допустим дубликат ключа
    {"banana", 3}
};

// Вставка элементов в unordered_multimap
umm.insert({"apple", 4});  // Добавляет новый элемент с тем же ключом

// Поиск элементов
auto it = um.find("apple");
if (it != um.end()) {
    std::cout << "Значение для apple: " << it->second << std::endl;
}

// Удаление элементов
um.erase("banana");  // Удаление по ключу
umm.erase("apple");  // Удаление всех элементов с ключом "apple"

// Проверка наличия ключа
if (um.count("orange") > 0) {
    std::cout << "Ключ orange найден" << std::endl;
}

// Резервирование места
um.reserve(1000);  // Предварительное выделение памяти
```

### Неправильное использование

```cpp
// Неправильно: предположение о порядке элементов
std::unordered_map<std::string, int> um = {
    {"apple", 1}, {"banana", 2}, {"orange", 3}
};
for (auto it = um.begin(); it != um.end(); ++it) {
    // Порядок элементов не гарантирован
    std::cout << it->first << ": " << it->second << std::endl;
}

// Правильно: использование map для отсортированного порядка
std::map<std::string, int> m = {
    {"apple", 1}, {"banana", 2}, {"orange", 3}
};
for (auto it = m.begin(); it != m.end(); ++it) {
    // Элементы гарантированно отсортированы по ключу
    std::cout << it->first << ": " << it->second << std::endl;
}

// Неправильно: частое изменение размера
std::unordered_map<int, int> um;
for (int i = 0; i < 1000; ++i) {
    um[i] = i;  // Может вызвать рехеширование
}

// Правильно: предварительное резервирование места
std::unordered_map<int, int> um;
um.reserve(1000);  // Предотвращает рехеширование
for (int i = 0; i < 1000; ++i) {
    um[i] = i;
}
```

## Рекомендации по использованию

1. Используйте `std::unordered_map` когда:
   - Нужны уникальные ключи
   - Требуется быстрый поиск по ключу
   - Порядок элементов не важен
   - Ключи поддерживают хеширование

2. Используйте `std::unordered_multimap` когда:
   - Допустимы дубликаты ключей
   - Требуется быстрый поиск по ключу
   - Порядок элементов не важен
   - Ключи поддерживают хеширование

3. Не используйте `unordered_map`/`unordered_multimap` когда:
   - Нужен отсортированный порядок элементов
   - Важна стабильность итераторов
   - Ключи не поддерживают хеширование
   - Нужна предсказуемая производительность

## Особенности реализации

```cpp
// Пример структуры хеш-таблицы для map
template<typename K, typename V>
class HashTable {
    struct Node {
        std::pair<K, V> data;
        Node* next;
    };
    
    std::vector<Node*> buckets;
    size_t size;
    
    size_t hash(const K& key) const {
        return std::hash<K>{}(key) % buckets.size();
    }
    
public:
    void insert(const K& key, const V& value) {
        size_t index = hash(key);
        Node* newNode = new Node{{key, value}, buckets[index]};
        buckets[index] = newNode;
        size++;
        
        // Рехеширование при необходимости
        if (size > buckets.size() * 0.75) {
            rehash();
        }
    }
    
    V* find(const K& key) {
        size_t index = hash(key);
        Node* current = buckets[index];
        while (current != nullptr) {
            if (current->data.first == key) {
                return &current->data.second;
            }
            current = current->next;
        }
        return nullptr;
    }
};
``` 