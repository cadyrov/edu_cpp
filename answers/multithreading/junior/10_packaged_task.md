# Как работает `packaged_task`?

`std::packaged_task` - это класс, который оборачивает вызываемый объект (функцию, лямбду и т.д.) и позволяет получить его результат через `std::future`. Полезен для отложенного выполнения задач и передачи их между потоками.

## Основные характеристики:
- Оборачивает вызываемый объект
- Возвращает future с результатом
- Можно перемещать, но нельзя копировать
- Поддерживает передачу исключений
- Позволяет отложить выполнение задачи

## Пример использования:

```cpp
#include <iostream>
#include <future>
#include <thread>
#include <queue>
#include <mutex>
#include <condition_variable>

class TaskQueue {
    std::queue<std::packaged_task<int()>> tasks;
    std::mutex mtx;
    std::condition_variable cv;
    bool done = false;
    
public:
    void add_task(std::packaged_task<int()>&& task) {
        std::lock_guard<std::mutex> lock(mtx);
        tasks.push(std::move(task));
        cv.notify_one();
    }
    
    void run() {
        while (true) {
            std::packaged_task<int()> task;
            {
                std::unique_lock<std::mutex> lock(mtx);
                cv.wait(lock, [this]{ return !tasks.empty() || done; });
                
                if (done && tasks.empty()) {
                    break;
                }
                
                task = std::move(tasks.front());
                tasks.pop();
            }
            task();
        }
    }
    
    void stop() {
        std::lock_guard<std::mutex> lock(mtx);
        done = true;
        cv.notify_all();
    }
};

int main() {
    TaskQueue queue;
    std::thread worker(&TaskQueue::run, &queue);
    
    // Создаем и добавляем задачи
    std::vector<std::future<int>> futures;
    for (int i = 0; i < 5; ++i) {
        std::packaged_task<int()> task([i]() {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            return i * i;
        });
        futures.push_back(task.get_future());
        queue.add_task(std::move(task));
    }
    
    // Получаем результаты
    for (int i = 0; i < 5; ++i) {
        std::cout << "Result " << i << ": " << futures[i].get() << std::endl;
    }
    
    queue.stop();
    worker.join();
    return 0;
}
```

## Как правильно использовать:
1. Используйте для отложенного выполнения задач
2. Передавайте задачи между потоками
3. Обрабатывайте исключения через future
4. Сохраняйте future до получения результата

## Как НЕ надо использовать:
```cpp
// ❌ Неправильно: Множественное выполнение задачи
std::packaged_task<int()> task([]() { return 42; });
task(); // Выполняем первый раз
task(); // Вызовет std::future_error

// ❌ Неправильно: Использование после перемещения
std::packaged_task<int()> task1([]() { return 42; });
auto task2 = std::move(task1);
task1(); // Вызовет std::future_error

// ❌ Неправильно: Игнорирование future
std::packaged_task<void()> task([]() {
    // Длительная операция
});
task(); // Результат будет потерян
```

## Комментарии:
- packaged_task удобен для реализации очередей задач
- Используйте для отложенного выполнения
- Помните о необходимости сохранения future
- Хорошо подходит для реализации thread pool 