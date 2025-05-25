# Как работает `mutex`?

`std::mutex` (mutual exclusion) - это примитив синхронизации, который используется для защиты общих данных от одновременного доступа нескольких потоков.

## Основные характеристики:
- Обеспечивает взаимное исключение доступа к общим ресурсам
- Только один поток может владеть мьютексом в один момент времени
- Другие потоки должны ждать освобождения мьютекса
- Предотвращает race conditions

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int sharedData = 0;

void increment() {
    for (int i = 0; i < 1000; ++i) {
        mtx.lock();
        sharedData++;
        mtx.unlock();
    }
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);
    
    t1.join();
    t2.join();
    
    std::cout << "Final value: " << sharedData << std::endl;
    return 0;
}
```

## Как правильно использовать:
1. Используйте RAII-обертки (lock_guard, unique_lock) вместо прямых вызовов lock/unlock
2. Минимизируйте время удержания мьютекса
3. Избегайте вложенных блокировок
4. Всегда освобождайте мьютекс в случае исключений

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Прямое использование lock/unlock
std::mutex mtx;
mtx.lock();
// ... код ...
if (error) {
    return; // Мьютекс не будет разблокирован!
}
mtx.unlock();

// ❌ Неправильно: Вложенные блокировки
std::mutex mtx1, mtx2;
std::thread t1([&]() {
    mtx1.lock();
    mtx2.lock();
    // ... код ...
    mtx2.unlock();
    mtx1.unlock();
});

// ❌ Неправильно: Долгое удержание мьютекса
std::mutex mtx;
mtx.lock();
// Длительная операция
std::this_thread::sleep_for(std::chrono::seconds(1));
mtx.unlock();
```

## Комментарии:
- Мьютексы могут стать узким местом в производительности
- При неправильном использовании могут привести к deadlock
- Для простых случаев лучше использовать atomic операции
- В сложных случаях рассмотрите использование read-write мьютексов (shared_mutex) 