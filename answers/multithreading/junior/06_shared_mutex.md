# Как работает `shared_mutex`?

`std::shared_mutex` - это мьютекс, который поддерживает два типа блокировок: shared (читающий) и exclusive (пишущий). Позволяет нескольким потокам одновременно читать данные, но только одному потоку писать.

## Основные характеристики:
- Поддерживает shared (читающий) и exclusive (пишущий) доступ
- Несколько потоков могут иметь shared блокировку одновременно
- Только один поток может иметь exclusive блокировку
- Нельзя получить shared блокировку, если есть exclusive
- Нельзя получить exclusive блокировку, если есть shared

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <shared_mutex>
#include <vector>

std::shared_mutex mtx;
std::vector<int> data = {1, 2, 3, 4, 5};

void reader(int id) {
    // Получаем shared блокировку
    mtx.lock_shared();
    std::cout << "Reader " << id << " sees: ";
    for (int x : data) {
        std::cout << x << " ";
    }
    std::cout << std::endl;
    mtx.unlock_shared();
}

void writer() {
    // Получаем exclusive блокировку
    mtx.lock();
    data.push_back(data.back() + 1);
    std::cout << "Writer added new value" << std::endl;
    mtx.unlock();
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
1. Используйте для данных с частым чтением и редкой записью
2. Применяйте RAII-обертки (shared_lock, unique_lock)
3. Минимизируйте время удержания блокировок
4. Следите за порядком блокировок для предотвращения deadlock

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Прямое использование lock/unlock
std::shared_mutex mtx;
mtx.lock_shared();
// ... код ...
if (error) {
    return; // Забыли unlock_shared!
}
mtx.unlock_shared();

// ❌ Неправильно: Смешивание типов блокировок
std::shared_mutex mtx;
mtx.lock(); // exclusive блокировка
// ... код ...
mtx.lock_shared(); // Попытка получить shared блокировку
// Deadlock!

// ❌ Неправильно: Долгое удержание exclusive блокировки
std::shared_mutex mtx;
mtx.lock();
// Длительная операция
std::this_thread::sleep_for(std::chrono::seconds(1));
mtx.unlock();
```

## Комментарии:
- shared_mutex эффективен для данных с частым чтением
- Не используйте для данных с частой записью
- Всегда используйте RAII-обертки вместо прямых вызовов
- Помните о возможности deadlock при использовании нескольких мьютексов 