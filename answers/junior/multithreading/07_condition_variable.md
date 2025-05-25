# Что такое `condition_variable`?

`std::condition_variable` - это примитив синхронизации, который позволяет потокам ждать, пока не будет выполнено определенное условие. Используется для реализации паттерна "производитель-потребитель" и других сценариев, где потоки должны ждать определенных событий.

## Основные характеристики:
- Позволяет потокам ждать выполнения условия
- Требует мьютекс для работы
- Поддерживает уведомление одного или всех ожидающих потоков
- Может работать с таймаутами

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

std::mutex mtx;
std::condition_variable cv;
std::queue<int> data;
bool done = false;

void producer() {
    for (int i = 0; i < 5; ++i) {
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
        std::unique_lock<std::mutex> lock(mtx);
        data.push(i);
        std::cout << "Produced: " << i << std::endl;
        cv.notify_one();
    }
    
    std::unique_lock<std::mutex> lock(mtx);
    done = true;
    cv.notify_all();
}

void consumer(int id) {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, []{ return !data.empty() || done; });
        
        if (done && data.empty()) {
            break;
        }
        
        int value = data.front();
        data.pop();
        std::cout << "Consumer " << id << " consumed: " << value << std::endl;
    }
}

int main() {
    std::thread prod(producer);
    std::thread cons1(consumer, 1);
    std::thread cons2(consumer, 2);
    
    prod.join();
    cons1.join();
    cons2.join();
    
    return 0;
}
```

## Как правильно использовать:
1. Всегда используйте с мьютексом
2. Проверяйте условие в цикле (spurious wakeups)
3. Используйте RAII-обертки для мьютекса
4. Минимизируйте время удержания блокировки

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Отсутствие проверки условия
std::condition_variable cv;
std::mutex mtx;
cv.wait(mtx); // Неправильно! Нужно передать unique_lock

// ❌ Неправильно: Неправильная проверка условия
std::condition_variable cv;
std::mutex mtx;
std::unique_lock<std::mutex> lock(mtx);
cv.wait(lock, []{ return true; }); // Бесконечное ожидание

// ❌ Неправильно: Уведомление без блокировки
std::condition_variable cv;
std::mutex mtx;
cv.notify_one(); // Небезопасно! Нужно заблокировать мьютекс
```

## Комментарии:
- condition_variable - мощный инструмент для синхронизации потоков
- Помните о spurious wakeups (ложных пробуждениях)
- Используйте для реализации сложных паттернов синхронизации
- Не забывайте о необходимости блокировки мьютекса при уведомлении 