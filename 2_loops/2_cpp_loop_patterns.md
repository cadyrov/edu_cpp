# Паттерны и алгоритмы в циклах

## Вывод через разделитель
- Количество разделителей на 1 меньше количества элементов
- Используется флаг первой итерации

```cpp
bool is_first = true;
while (std::cin >> word) {
    if (!is_first) {
        std::cout << ", ";
    }
    is_first = false;
    std::cout << word;
}
```

## Аккумулятор
- Переменная для накопления результата
- Важно правильно выбрать начальное значение

### Примеры
```cpp
// Сумма чисел
int sum = 0;  // Начальное значение для сложения
for (int i = 0; i < n; ++i) {
    int num;
    std::cin >> num;
    sum += num;
}

// Произведение чисел
int product = 1;  // Начальное значение для умножения
for (int i = 0; i < n; ++i) {
    int num;
    std::cin >> num;
    product *= num;
}
```

## Поиск максимума/минимума
- Используется std::max/std::min из <algorithm>
- Начальное значение должно быть корректным

```cpp
int num;
std::cin >> num;
int cur_max = num;  // Инициализируем первым числом

for (int i = 1; i < n; ++i) {
    std::cin >> num;
    cur_max = std::max(cur_max, num);
}
```

## Поиск k-го по величине
- Храним k наибольших элементов
- Обновляем их при чтении новых чисел

```cpp
// Поиск второго по величине
int num1, num2;
std::cin >> num1 >> num2;
int cur_max = std::max(num1, num2);
int cur_next = std::min(num1, num2);

for (int i = 2; i < n; ++i) {
    int num;
    std::cin >> num;
    if (num > cur_max) {
        cur_next = cur_max;
        cur_max = num;
    } else if (num > cur_next) {
        cur_next = num;
    }
}
```

## Конечный автомат (State Machine)
- Использует переменную состояния
- Поведение зависит от текущего состояния

```cpp
bool is_after_backslash = false;
for (size_t i = 0; i < str.size(); ++i) {
    if (!is_after_backslash) {
        if (str[i] != '\\') {
            std::cout << str[i];
        } else {
            is_after_backslash = true;
        }
    } else {
        if (str[i] != '\\') {
            std::cerr << "Ошибка: \\" << str[i] << std::endl;
        } else {
            std::cout << '\\';
        }
        is_after_backslash = false;
    }
}
```

## Важные моменты
1. Используйте инварианты для проверки корректности алгоритма
2. Правильно выбирайте начальные значения для аккумуляторов
3. При работе с числами с плавающей точкой учитывайте порядок операций
4. Для поиска максимума/минимума инициализируйте переменную первым элементом
5. В конечном автомате четко определите переходы между состояниями 