# Как работает `bitset`?

## Ответ
`std::bitset` - это контейнер фиксированного размера для хранения последовательности битов. Он оптимизирован для работы с битовыми операциями и предоставляет удобный интерфейс для манипуляции отдельными битами.

Основные характеристики:
1. Фиксированный размер, задаваемый на этапе компиляции
2. Эффективное хранение (1 бит на элемент)
3. Поддержка битовых операций (AND, OR, XOR, NOT)
4. Поддержка сдвигов и проверок
5. Конвертация в/из строк

## Примеры кода

### Правильное использование

```cpp
// Создание bitset размером 8 бит
std::bitset<8> b1(42);  // 00101010
std::bitset<8> b2("10101010");  // Из строки

// Доступ к отдельным битам
bool bit = b1[0];  // Получение значения бита
b1[0] = true;      // Установка бита

// Битовая арифметика
std::bitset<8> b3 = b1 & b2;  // Побитовое AND
std::bitset<8> b4 = b1 | b2;  // Побитовое OR
std::bitset<8> b5 = b1 ^ b2;  // Побитовое XOR
std::bitset<8> b6 = ~b1;      // Побитовое NOT

// Сдвиги
std::bitset<8> b7 = b1 << 2;  // Сдвиг влево
std::bitset<8> b8 = b1 >> 2;  // Сдвиг вправо

// Проверки
bool all_set = b1.all();      // Все биты установлены?
bool any_set = b1.any();      // Хотя бы один бит установлен?
bool none_set = b1.none();    // Ни один бит не установлен?
size_t count = b1.count();    // Количество установленных битов

// Конвертация
std::string str = b1.to_string();  // В строку
unsigned long val = b1.to_ulong(); // В число
```

### Неправильное использование

```cpp
// Неправильно: попытка изменить размер
std::bitset<8> b1;
b1.resize(16);  // Ошибка! Размер фиксирован

// Неправильно: использование неверного размера при создании из числа
std::bitset<4> b2(42);  // Теряем старшие биты

// Неправильно: использование неверного формата строки
std::bitset<8> b3("12345");  // Ошибка! Только '0' и '1'

// Правильно: создание с правильным размером
std::bitset<8> b4(42);  // 00101010
std::bitset<8> b5("00101010");  // Из строки с правильным форматом

// Неправильно: попытка использовать bitset для хранения больших данных
std::bitset<1000000> big_bitset;  // Неэффективно для больших размеров

// Правильно: использование vector<bool> для больших данных
std::vector<bool> v(1000000);
```

## Рекомендации по использованию

1. Используйте `std::bitset` когда:
   - Нужно работать с фиксированным набором флагов
   - Требуется эффективное хранение булевых значений
   - Нужны битовые операции
   - Размер известен на этапе компиляции
   - Важна эффективность битовых операций

2. Не используйте `std::bitset` когда:
   - Размер не известен на этапе компиляции
   - Нужно динамически изменять размер
   - Требуется хранение большого количества битов
   - Нужна гибкость в работе с данными

## Особенности реализации

```cpp
// Пример реализации bitset
template<size_t N>
class Bitset {
    static constexpr size_t BITS_PER_WORD = 8 * sizeof(unsigned long);
    static constexpr size_t WORD_COUNT = (N + BITS_PER_WORD - 1) / BITS_PER_WORD;
    
    unsigned long data[WORD_COUNT] = {0};
    
    size_t word_index(size_t pos) const {
        return pos / BITS_PER_WORD;
    }
    
    size_t bit_index(size_t pos) const {
        return pos % BITS_PER_WORD;
    }
    
public:
    // Конструкторы
    Bitset() = default;
    Bitset(unsigned long val) {
        data[0] = val;
    }
    
    // Доступ к битам
    bool operator[](size_t pos) const {
        return (data[word_index(pos)] & (1UL << bit_index(pos))) != 0;
    }
    
    // Установка/сброс битов
    void set(size_t pos, bool value = true) {
        if (value) {
            data[word_index(pos)] |= (1UL << bit_index(pos));
        } else {
            data[word_index(pos)] &= ~(1UL << bit_index(pos));
        }
    }
    
    void reset(size_t pos) {
        set(pos, false);
    }
    
    // Битовая арифметика
    Bitset operator&(const Bitset& other) const {
        Bitset result;
        for (size_t i = 0; i < WORD_COUNT; ++i) {
            result.data[i] = data[i] & other.data[i];
        }
        return result;
    }
    
    // Проверки
    bool all() const {
        for (size_t i = 0; i < N; ++i) {
            if (!(*this)[i]) return false;
        }
        return true;
    }
    
    bool any() const {
        for (size_t i = 0; i < N; ++i) {
            if ((*this)[i]) return true;
        }
        return false;
    }
    
    size_t count() const {
        size_t result = 0;
        for (size_t i = 0; i < N; ++i) {
            if ((*this)[i]) ++result;
        }
        return result;
    }
};
``` 