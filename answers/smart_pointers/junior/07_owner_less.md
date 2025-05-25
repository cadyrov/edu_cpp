# Что такое `owner_less`?

## Ответ
`owner_less` - это функциональный объект, который сравнивает умные указатели по их владельцу (то есть по указателю на управляемый объект), а не по значению указателя. Это полезно при использовании `shared_ptr` и `weak_ptr` в ассоциативных контейнерах, где нужно сравнивать указатели по их владельцу, а не по их значению.

## Примеры кода

### Правильное использование:
```cpp
// Создание множества shared_ptr с owner_less
std::set<std::shared_ptr<int>, std::owner_less<std::shared_ptr<int>>> ptrSet;

// Добавление указателей
auto ptr1 = std::make_shared<int>(42);
auto ptr2 = std::make_shared<int>(42);
ptrSet.insert(ptr1);
ptrSet.insert(ptr2); // Добавится, так как это разные объекты

// Создание weak_ptr
std::weak_ptr<int> weak = ptr1;
// Проверка наличия weak_ptr в множестве
bool exists = ptrSet.find(weak) != ptrSet.end(); // Работает благодаря owner_less

// Использование в map
std::map<std::shared_ptr<int>, int, std::owner_less<std::shared_ptr<int>>> ptrMap;
ptrMap[ptr1] = 1;
ptrMap[ptr2] = 2;
```

### Неправильное использование:
```cpp
// Ошибка: использование обычного сравнения
std::set<std::shared_ptr<int>> wrongSet;
std::weak_ptr<int> weak = ptr1;
wrongSet.find(weak); // Ошибка компиляции: нельзя сравнивать shared_ptr и weak_ptr

// Ошибка: неправильное использование owner_less
std::set<std::unique_ptr<int>, std::owner_less<std::unique_ptr<int>>> uniqueSet; // Ошибка: owner_less не работает с unique_ptr

// Ошибка: смешивание разных типов указателей
std::set<std::shared_ptr<int>, std::owner_less<std::weak_ptr<int>>> mixedSet; // Ошибка: несоответствие типов
```

## Комментарии
- `owner_less` позволяет сравнивать `shared_ptr` и `weak_ptr` по их владельцу
- Полезен при использовании умных указателей в ассоциативных контейнерах
- Работает только с `shared_ptr` и `weak_ptr`
- Не работает с `unique_ptr`
- Упрощает работу с контейнерами, содержащими умные указатели 