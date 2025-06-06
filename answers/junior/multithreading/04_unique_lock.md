# Как использовать `unique_lock`?

`std::unique_lock` - это более гибкая RAII-обертка для мьютекса, чем `lock_guard`. Она позволяет отложить блокировку, разблокировать мьютекс вручную и использовать условные переменные.

## Основные характеристики:
- Можно отложить блокировку при создании
- Можно разблокировать и заблокировать мьютекс вручную
- Поддерживает условные переменные
- Можно перемещать, но нельзя копировать
- Более гибкий, но и более тяжелый чем lock_guard

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int sharedData = 0;

void increment() {
    std::unique_lock<std::mutex> lock(mtx, std::defer_lock);
    
    // Можно выполнить какую-то работу до блокировки
    std::cout << "Doing some work before locking" << std::endl;
    
    // Блокируем мьютекс когда нужно
    lock.lock();
    sharedData++;
    // Можно разблокировать и заблокировать снова
    lock.unlock();
    
    // Какая-то работа без блокировки
    std::cout << "Doing some work without lock" << std::endl;
    
    // Блокируем снова
    lock.lock();
    sharedData++;
}
```

## Как правильно использовать:
1. Используйте для сложных сценариев синхронизации
2. Применяйте с условными переменными
3. Используйте для случаев, требующих отложенной блокировки
4. Комбинируйте с другими примитивами синхронизации

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Использование для простых случаев
// Лучше использовать lock_guard
std::unique_lock<std::mutex> lock(mtx);
sharedData++;

// ❌ Неправильно: Потеря блокировки
std::unique_lock<std::mutex> lock(mtx);
lock.unlock();
// Забыли заблокировать снова
sharedData++; // Небезопасно!

// ❌ Неправильно: Слишком частое переключение блокировки
std::unique_lock<std::mutex> lock(mtx);
for (int i = 0; i < 1000; ++i) {
    lock.unlock();
    // Какая-то работа
    lock.lock();
    sharedData++;
}
```

## Комментарии:
- unique_lock более гибкий, но и более тяжелый чем lock_guard
- Используйте только когда нужна дополнительная функциональность
- Хорошо подходит для работы с условными переменными
- Позволяет реализовать более сложные паттерны синхронизации 