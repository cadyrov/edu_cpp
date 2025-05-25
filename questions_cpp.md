# Вопросы для собеседования по C++17

## 1. Основы C++17 (60 вопросов)

### Junior (20 вопросов)
1. Что такое C++17? Какие основные нововведения он принес?
2. В чем разница между `auto` и `decltype`?
3. Что такое `nullptr` и почему он лучше `NULL`?
4. Объясните разницу между `const` и `constexpr`
5. Что такое `inline` переменные в C++17?
6. Как работает `if constexpr`?
7. Что такое структурированные привязки (structured bindings)?
8. Объясните концепцию `if` и `switch` с инициализатором
9. Что такое `[[nodiscard]]`, `[[maybe_unused]]`, `[[fallthrough]]`?
10. Как работает `std::optional`?
11. Что такое `std::variant`?
12. Как использовать `std::any`?
13. Что такое `std::string_view`?
14. Как работает `std::filesystem`?
15. Что такое `std::invoke`?
16. Как использовать `std::apply`?
17. Что такое `std::tuple` и как с ним работать?
18. Как работает `std::tie`?
19. Что такое `std::pair`?
20. Как использовать `std::map` и `std::unordered_map`?

### Middle (20 вопросов)
1. Что такое концепты (concepts) в C++17?
2. Как работает `if constexpr` с концептами?
3. Что такое `std::invoke_result`?
4. Как использовать `std::is_invocable`?
5. Что такое `std::void_t`?
6. Как работает `std::conjunction` и `std::disjunction`?
7. Что такое `std::is_constant_evaluated`?
8. Как использовать `std::has_unique_object_representations`?
9. Что такое `std::is_aggregate`?
10. Как работает `std::is_swappable`?
11. Что такое `std::is_nothrow_swappable`?
12. Как использовать `std::is_constructible`?
13. Что такое `std::is_trivially_constructible`?
14. Как работает `std::is_assignable`?
15. Что такое `std::is_trivially_assignable`?
16. Как использовать `std::is_destructible`?
17. Что такое `std::is_trivially_destructible`?
18. Как работает `std::is_copy_constructible`?
19. Что такое `std::is_move_constructible`?
20. Как использовать `std::is_copy_assignable`?

### Senior (20 вопросов)
1. Как реализовать свой концепт?
2. Что такое SFINAE и как его использовать?
3. Как работает perfect forwarding?
4. Что такое move semantics и как их правильно использовать?
5. Как реализовать свой тип с move semantics?
6. Что такое copy elision и RVO?
7. Как работает `std::launder`?
8. Что такое `std::hardware_destructive_interference_size`?
9. Как использовать `std::hardware_constructive_interference_size`?
10. Что такое `std::scoped_lock`?
11. Как работает `std::shared_mutex`?
12. Что такое `std::shared_lock`?
13. Как использовать `std::unique_lock`?
14. Что такое `std::lock_guard`?
15. Как работает `std::mutex`?
16. Что такое `std::condition_variable`?
17. Как использовать `std::condition_variable_any`?
18. Что такое `std::promise` и `std::future`?
19. Как работает `std::async`?
20. Что такое `std::packaged_task`?

## 2. ООП (45 вопросов)

### Junior (15 вопросов)
1. Что такое класс и объект?
2. В чем разница между `struct` и `class`?
3. Что такое конструктор и деструктор?
4. Как работает наследование?
5. Что такое виртуальные функции?
6. Как работает полиморфизм?
7. Что такое абстрактный класс?
8. Как использовать `override` и `final`?
9. Что такое `friend` функции и классы?
10. Как работает `static` в классах?
11. Что такое `const` методы?
12. Как использовать `mutable`?
13. Что такое `explicit` конструктор?
14. Как работает `operator overloading`?
15. Что такое `this` указатель?

### Middle (15 вопросов)
1. Что такое CRTP (Curiously Recurring Template Pattern)?
2. Как реализовать singleton?
3. Что такое RAII?
4. Как работает PIMPL idiom?
5. Что такое virtual inheritance?
6. Как использовать `std::enable_shared_from_this`?
7. Что такое `std::is_base_of`?
8. Как работает `std::is_convertible`?
9. Что такое `std::is_polymorphic`?
10. Как использовать `std::is_abstract`?
11. Что такое `std::is_final`?
12. Как работает `std::is_empty`?
13. Что такое `std::is_standard_layout`?
14. Как использовать `std::is_pod`?
15. Что такое `std::is_trivial`?

### Senior (15 вопросов)
1. Как реализовать свой allocator?
2. Что такое type erasure?
3. Как работает expression templates?
4. Что такое policy-based design?
5. Как реализовать visitor pattern?
6. Что такое double dispatch?
7. Как работает mixin?
8. Что такое CRTP с multiple inheritance?
9. Как реализовать type-safe enum?
10. Что такое type traits?
11. Как работает SFINAE в контексте ООП?
12. Что такое virtual inheritance diamond?
13. Как реализовать multiple dispatch?
14. Что такое static polymorphism?
15. Как работает type erasure с templates?

## 3. STL Контейнеры (60 вопросов)

### Junior (20 вопросов)
1. Какие последовательные контейнеры есть в STL?
2. В чем разница между `vector` и `array`?
3. Как работает `list`?
4. Что такое `deque`?
5. Как использовать `forward_list`?
6. Что такое `set` и `multiset`?
7. Как работает `map` и `multimap`?
8. Что такое `unordered_set` и `unordered_multiset`?
9. Как использовать `unordered_map` и `unordered_multimap`?
10. Что такое `stack` и `queue`?
11. Как работает `priority_queue`?
12. Что такое `bitset`?
13. Как использовать `valarray`?
14. Что такое `string`?
15. Как работает `string_view`?
16. Что такое `span`?
17. Как использовать `array`?
18. Что такое `vector<bool>`?
19. Как работает `initializer_list`?
20. Что такое `pair` и `tuple`?

### Middle (20 вопросов)
1. Как реализовать свой контейнер?
2. Что такое allocator?
3. Как работает `emplace`?
4. Что такое `try_emplace`?
5. Как использовать `insert_or_assign`?
6. Что такое `extract`?
7. Как работает `merge`?
8. Что такое `splice`?
9. Как использовать `unique`?
10. Что такое `sort`?
11. Как работает `stable_sort`?
12. Что такое `partial_sort`?
13. Как использовать `nth_element`?
14. Что такое `inplace_merge`?
15. Как работает `includes`?
16. Что такое `set_union`?
17. Как использовать `set_intersection`?
18. Что такое `set_difference`?
19. Как работает `set_symmetric_difference`?
20. Что такое `merge`?

### Senior (20 вопросов)
1. Как реализовать thread-safe контейнер?
2. Что такое lock-free контейнеры?
3. Как работает memory model в контексте контейнеров?
4. Что такое custom allocator?
5. Как реализовать pool allocator?
6. Что такое arena allocator?
7. Как работает memory pool?
8. Что такое custom deleter?
9. Как реализовать custom comparator?
10. Что такое custom hash function?
11. Как работает custom key extractor?
12. Что такое custom value extractor?
13. Как реализовать custom iterator?
14. Что такое custom sentinel?
15. Как работает custom range?
16. Что такое custom view?
17. Как реализовать custom algorithm?
18. Что такое custom predicate?
19. Как работает custom projection?
20. Что такое custom comparator для partial ordering?

## 4. Итераторы и алгоритмы (45 вопросов)

### Junior (15 вопросов)
1. Что такое итератор?
2. Какие типы итераторов есть в STL?
3. Как работает `begin()` и `end()`?
4. Что такое `rbegin()` и `rend()`?
5. Как использовать `cbegin()` и `cend()`?
6. Что такое `crbegin()` и `crend()`?
7. Как работает `find()`?
8. Что такое `find_if()`?
9. Как использовать `count()`?
10. Что такое `count_if()`?
11. Как работает `sort()`?
12. Что такое `stable_sort()`?
13. Как использовать `binary_search()`?
14. Что такое `lower_bound()`?
15. Как работает `upper_bound()`?

### Middle (15 вопросов)
1. Как реализовать свой итератор?
2. Что такое iterator traits?
3. Как работает iterator categories?
4. Что такое iterator concepts?
5. Как использовать iterator adaptors?
6. Что такое stream iterators?
7. Как работает move iterators?
8. Что такое reverse iterators?
9. Как использовать insert iterators?
10. Что такое front insert iterators?
11. Как работает back insert iterators?
12. Что такое istream iterators?
13. Как использовать ostream iterators?
14. Что такое istreambuf iterators?
15. Как работает ostreambuf iterators?

### Senior (15 вопросов)
1. Как реализовать parallel algorithms?
2. Что такое execution policies?
3. Как работает parallel execution?
4. Что такое vectorized execution?
5. Как реализовать custom execution policy?
6. Что такое custom algorithm?
7. Как работает custom predicate?
8. Что такое custom projection?
9. Как реализовать custom comparator?
10. Что такое custom sentinel?
11. Как работает custom range?
12. Что такое custom view?
13. Как реализовать custom iterator?
14. Что такое custom allocator?
15. Как работает custom deleter?

## 5. Умные указатели и управление памятью (30 вопросов)

### Junior (10 вопросов)
1. Что такое `unique_ptr`?
2. Как работает `shared_ptr`?
3. Что такое `weak_ptr`?
4. Как использовать `make_unique`?
5. Что такое `make_shared`?
6. Как работает `enable_shared_from_this`?
7. Что такое `owner_less`?
8. Как использовать `pointer_traits`?
9. Что такое `allocator_traits`?
10. Как работает `scoped_allocator_adaptor`?

### Middle (10 вопросов)
1. Как реализовать свой умный указатель?
2. Что такое custom deleter?
3. Как работает custom allocator?
4. Что такое custom pointer traits?
5. Как использовать custom allocator traits?
6. Что такое custom scoped allocator adaptor?
7. Как работает custom owner less?
8. Что такое custom enable shared from this?
9. Как использовать custom make unique?
10. Что такое custom make shared?

### Senior (10 вопросов)
1. Как реализовать thread-safe умный указатель?
2. Что такое lock-free умные указатели?
3. Как работает memory model в контексте умных указателей?
4. Что такое custom memory model?
5. Как реализовать custom memory order?
6. Что такое custom atomic operations?
7. Как работает custom memory fence?
8. Что такое custom memory barrier?
9. Как реализовать custom memory pool?
10. Что такое custom arena allocator?

## 6. Строки и регулярные выражения (30 вопросов)

### Junior (10 вопросов)
1. Что такое `string`?
2. Как работает `string_view`?
3. Что такое `regex`?
4. Как использовать `regex_match`?
5. Что такое `regex_search`?
6. Как работает `regex_replace`?
7. Что такое `regex_iterator`?
8. Как использовать `regex_token_iterator`?
9. Что такое `sub_match`?
10. Как работает `match_results`?

### Middle (10 вопросов)
1. Как реализовать свой string?
2. Что такое custom string traits?
3. Как работает custom string allocator?
4. Что такое custom string view?
5. Как использовать custom regex?
6. Что такое custom regex traits?
7. Как работает custom regex iterator?
8. Что такое custom regex token iterator?
9. Как использовать custom sub match?
10. Что такое custom match results?

### Senior (10 вопросов)
1. Как реализовать thread-safe string?
2. Что такое lock-free string?
3. Как работает memory model в контексте строк?
4. Что такое custom memory model для строк?
5. Как реализовать custom memory order для строк?
6. Что такое custom atomic operations для строк?
7. Как работает custom memory fence для строк?
8. Что такое custom memory barrier для строк?
9. Как реализовать custom memory pool для строк?
10. Что такое custom arena allocator для строк?

## 7. Многопоточность и асинхронность (30 вопросов)

### Junior (10 вопросов)
1. Что такое `thread`?
2. Как работает `mutex`?
3. Что такое `lock_guard`?
4. Как использовать `unique_lock`?
5. Что такое `shared_lock`?
6. Как работает `shared_mutex`?
7. Что такое `condition_variable`?
8. Как использовать `future` и `promise`?
9. Что такое `async`?
10. Как работает `packaged_task`?

### Middle (10 вопросов)
1. Как реализовать thread pool?
2. Что такое work stealing?
3. Как работает task scheduling?
4. Что такое custom scheduler?
5. Как использовать custom executor?
6. Что такое custom thread?
7. Как работает custom mutex?
8. Что такое custom lock?
9. Как использовать custom condition variable?
10. Что такое custom future?

### Senior (10 вопросов)
1. Как реализовать lock-free алгоритмы?
2. Что такое memory model?
3. Как работает atomic operations?
4. Что такое memory ordering?
5. Как реализовать custom memory model?
6. Что такое custom atomic operations?
7. Как работает custom memory ordering?
8. Что такое custom memory fence?
9. Как реализовать custom memory barrier?
10. Что такое custom memory pool? 