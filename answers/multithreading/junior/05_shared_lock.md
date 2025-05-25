# Что такое `shared_lock`?

`std::shared_lock` - это RAII-обертка для shared_mutex, которая обеспечивает shared (читающий) доступ к защищенным данным. Позволяет нескольким потокам одновременно читать данные, но только одному потоку писать.

## Основные характеристики:
- Обеспечивает shared (читающий) доступ к данным
- Несколько потоков могут одновременно иметь shared блокировку
- Нельзя получить shared блокировку, если есть exclusive блокировка
- Нельзя получить exclusive блокировку, если есть shared блокировка

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <shared_mutex>
#include <vector>

std::shared_mutex mtx;
std::vector<int> data = {1, 2, 3, 4, 5};

void reader(int id) {
    std::shared_lock<std::shared_mutex> lock(mtx);
    std::cout << "Reader " << id << " sees: ";
    for (int x : data) {
        std::cout << x << " ";
    }
    std::cout << std::endl;
}

void writer() {
    std::unique_lock<std::shared_mutex> lock(mtx);
    data.push_back(data.back() + 1);
    std::cout << "Writer added new value" << std::endl;
}

int main() {
    std::thread readers[3];
    for (int i = 0; i < 3; ++i) {
        readers[i] = std::thread(reader, i);
    }
    
    std::thread writer_thread(writer);
    
    for (auto& t : readers) {
        t.join();
    }
    writer_thread.join();
    
    return 0;
}
```

## Как правильно использовать:
1. Используйте для защиты данных, которые часто читаются, но редко изменяются
2. Применяйте в случаях, когда несколько потоков могут одновременно читать данные
3. Комбинируйте с unique_lock для записи
4. Минимизируйте время удержания блокировки

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Использование для записи
std::shared_lock<std::shared_mutex> lock(mtx);
data.push_back(42); // Небезопасно! shared_lock не защищает от записи

// ❌ Неправильно: Долгое удержание блокировки
std::shared_lock<std::shared_mutex> lock(mtx);
// Длительная операция чтения
std::this_thread::sleep_for(std::chrono::seconds(1));
// Блокирует других писателей

// ❌ Неправильно: Вложенные блокировки
std::shared_mutex mtx1, mtx2;
std::shared_lock<std::shared_mutex> lock1(mtx1);
std::shared_lock<std::shared_mutex> lock2(mtx2);
// Может привести к deadlock
```

## Комментарии:
- shared_lock эффективен для данных с частым чтением и редкой записью
- Не используйте для защиты данных, которые часто изменяются
- Убедитесь, что все потоки используют правильный тип блокировки
- Помните о возможности deadlock при использовании нескольких мьютексов 