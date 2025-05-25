# Как использовать `future` и `promise`?

`std::future` и `std::promise` - это механизм для асинхронного получения результата из потока. `promise` используется для установки значения, а `future` - для его получения.

## Основные характеристики:
- future позволяет получить результат асинхронной операции
- promise используется для установки значения
- Поддерживает передачу исключений между потоками
- Можно использовать для синхронизации потоков

## Пример использования:

```cpp
#include <iostream>
#include <thread>
#include <future>
#include <chrono>

int calculate(int x) {
    std::this_thread::sleep_for(std::chrono::seconds(1));
    return x * x;
}

int main() {
    // Создаем promise и получаем future
    std::promise<int> p;
    std::future<int> f = p.get_future();
    
    // Запускаем поток с promise
    std::thread t([&p]() {
        try {
            int result = calculate(5);
            p.set_value(result);
        } catch (...) {
            p.set_exception(std::current_exception());
        }
    });
    
    // Ждем результат в основном потоке
    try {
        int result = f.get();
        std::cout << "Result: " << result << std::endl;
    } catch (const std::exception& e) {
        std::cout << "Exception: " << e.what() << std::endl;
    }
    
    t.join();
    return 0;
}
```

## Как правильно использовать:
1. Используйте для получения результатов асинхронных операций
2. Обрабатывайте исключения через promise/future
3. Используйте wait_for/wait_until для таймаутов
4. Проверяйте валидность future перед использованием

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Множественное получение значения
std::promise<int> p;
std::future<int> f = p.get_future();
p.set_value(42);
int v1 = f.get();
int v2 = f.get(); // Вызовет std::future_error

// ❌ Неправильно: Использование невалидного future
std::future<int> f;
int v = f.get(); // Вызовет std::future_error

// ❌ Неправильно: Отсутствие обработки исключений
std::promise<int> p;
std::future<int> f = p.get_future();
std::thread t([&p]() {
    throw std::runtime_error("Error");
    p.set_value(42); // Не будет вызвано
});
t.join();
int v = f.get(); // Вызовет исключение
```

## Комментарии:
- future/promise - удобный механизм для асинхронного программирования
- Используйте для передачи результатов между потоками
- Помните о необходимости обработки исключений
- Не забывайте проверять валидность future 