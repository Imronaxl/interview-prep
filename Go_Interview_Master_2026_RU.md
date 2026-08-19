# Go Interview Master 2026 — Must Know / Good to Know

> Практический русскоязычный конспект для подготовки к собеседованию Go Backend Developer.
>
> Версия базы: **Go 1.26.x** (актуальная ветка на август 2026). Материал собран и нормализован по официальной документации Go и актуальным GitHub-репозиториям с интервью, roadmap, internals, стилем и практикой.

---

## 0. Как пользоваться этим файлом

### Уровни

- 🔴 **MUST KNOW** — без этого идти на Go backend-интервью рискованно.
- 🟡 **GOOD TO KNOW** — часто отличает сильного Junior/Middle от кандидата, знающего только синтаксис.
- 🟣 **DEEP DIVE** — для Middle+, highload, инфраструктурных команд и вопросов «а как это работает внутри?».

### Приоритет подготовки

1. Go basics + типы + slices/maps/interfaces.
2. Ошибки + `defer`/`panic`/`recover`.
3. Goroutines + channels + `select`.
4. `sync`, `sync/atomic`, race detector и memory model.
5. Context + graceful shutdown.
6. Testing + benchmarks + fuzzing.
7. HTTP/REST + JSON + middleware.
8. SQL/PostgreSQL + transactions + connection pool.
9. Go modules + tooling.
10. Runtime: scheduler, GC, stack/heap, escape analysis.
11. Profiling/observability.
12. gRPC, Kafka/message brokers, caching, distributed systems.
13. System design.
14. Практические coding-задачи.

---

# 1. Roadmap: от нуля до собеседования

## Stage 1 — Язык

### MUST KNOW

- `package`, `import`, `func`, `var`, `const`
- базовые типы
- zero values
- `:=`
- указатели
- arrays
- slices
- maps
- structs
- methods
- interfaces
- type assertions
- type switches
- `for`, `range`, `if`, `switch`
- multiple return values
- named returns
- closures
- variadic functions
- packages
- visibility: exported/unexported
- generics на базовом уровне

### GOOD TO KNOW

- method sets
- pointer/value receivers
- interface nil traps
- comparability
- `unsafe`
- reflection
- `go:generate`
- build tags

---

## Stage 2 — Ошибки и управление ресурсами

### MUST KNOW

- `error`
- `errors.New`
- `fmt.Errorf`
- `%w`
- `errors.Is`
- `errors.As`
- custom errors
- wrapping
- `defer`
- порядок выполнения defer
- аргументы defer
- `panic`
- `recover`
- почему обычный flow нельзя строить на panic

### GOOD TO KNOW

- error taxonomy
- sentinel errors
- typed errors
- stack traces
- обработка ошибок на границах приложения

---

## Stage 3 — Concurrency

### MUST KNOW

- goroutine
- channel
- buffered/unbuffered channel
- send/receive
- close
- `range` по channel
- `select`
- directional channels
- deadlock
- race condition
- data race
- goroutine leak
- `sync.WaitGroup`
- `sync.Mutex`
- `sync.RWMutex`
- `sync.Once`
- `sync.Cond`
- `sync/atomic`
- context cancellation
- worker pool
- fan-in
- fan-out
- pipeline

### GOOD TO KNOW

- happens-before
- memory model
- scheduler G/M/P
- work stealing
- preemption
- starvation
- livelock
- bounded concurrency
- semaphore
- `errgroup`
- cancellation propagation

---

## Stage 4 — Runtime

### MUST KNOW

- stack vs heap на концептуальном уровне
- garbage collector
- allocations
- escape analysis
- стоимость аллокаций
- почему `make([]T, 0, n)` иногда лучше
- почему `strings.Builder` полезен

### GOOD TO KNOW

- GC phases
- write barrier
- tri-color marking
- goroutine stacks
- stack growth
- scheduler
- GOMAXPROCS
- CPU vs I/O bound
- pprof
- trace

### DEEP DIVE

- runtime scheduler internals
- GC pacer
- assist/background GC
- stack copying
- compiler inlining
- SSA
- bounds-check elimination

---

## Stage 5 — Tooling

### MUST KNOW

```bash
go mod init
go mod tidy
go get
go test
go test ./...
go test -race ./...
go test -cover ./...
go vet ./...
go fmt ./...
go build
go run
go install
```

Также:

- `go.mod`
- `go.sum`
- modules
- semantic import versioning
- dependency management
- `gofmt`
- `go vet`

### GOOD TO KNOW

- `go list`
- `go env`
- `go work`
- vendoring
- `replace`
- build constraints
- toolchain management
- `go fix`

---

## Stage 6 — Testing

### MUST KNOW

- unit tests
- table-driven tests
- subtests
- `testing.T`
- test isolation
- mocks/fakes/stubs
- integration tests
- HTTP handler tests
- benchmarks
- race detector

### GOOD TO KNOW

- fuzz testing
- `httptest`
- `t.Parallel()`
- test fixtures
- deterministic tests
- testing concurrent code
- testing cancellation/timeouts

---

## Stage 7 — Backend

### MUST KNOW

- HTTP
- REST
- JSON
- HTTP status codes
- middleware
- routing
- authentication basics
- authorization basics
- timeouts
- request cancellation
- graceful shutdown
- connection pooling
- SQL transactions
- indexes
- isolation levels
- migrations
- caching

### GOOD TO KNOW

- gRPC
- protobuf
- Kafka
- RabbitMQ
- Redis
- idempotency
- retries
- backoff
- circuit breaker
- rate limiting
- distributed tracing

---

## Stage 8 — Production Go

### MUST KNOW

- structured logging
- metrics
- tracing
- health/readiness endpoints
- configuration
- secrets
- Docker
- CI/CD
- graceful shutdown
- resource limits
- observability

### GOOD TO KNOW

- OpenTelemetry
- Prometheus
- Kubernetes
- profiling in production
- pprof
- load testing
- SLO/SLI

---

# 2. MUST KNOW — вопросы и ответы

## 2.1 Основы Go

### Q1. Что такое Go?

**Ответ:** Go — компилируемый статически типизированный язык общего назначения с garbage collector и встроенной моделью конкурентного программирования. Он особенно популярен для backend, сетевых сервисов, инфраструктурных инструментов и микросервисов.

**На собеседовании:** не говори просто «Go быстрый». Лучше назвать причины: простой язык, сильная стандартная библиотека, быстрый toolchain, goroutines/channels, предсказуемая модель ошибок и хорошая производительность.

---

### Q2. Почему Go часто используют для backend?

**Ответ:** потому что у него сильная стандартная библиотека, дешёвые goroutines, удобная работа с сетью, простой deployment одного бинарника, хорошая производительность и понятный tooling.

---

### Q3. Go компилируемый или интерпретируемый?

**Ответ:** Go компилируется в машинный код. Компилятор также выполняет оптимизации, включая inlining и анализ escape.

---

### Q4. Что такое zero value?

**Ответ:** значение типа, которое Go автоматически получает без явной инициализации.

Примеры:

```go
var n int        // 0
var ok bool      // false
var s string     // ""
var p *int       // nil
var xs []int      // nil
var m map[string]int // nil
```

Идея Go: zero value по возможности должна быть полезной.

---

### Q5. `var x int` и `x := 10` — разница?

**Ответ:**

```go
var x int = 10
x := 10
```

`:=` — короткое объявление локальной переменной с выводом типа. Его нельзя использовать на уровне package scope и нельзя использовать без хотя бы одной новой переменной в соответствующем scope.

---

### Q6. Чем array отличается от slice?

**Ответ:** array имеет фиксированную длину и длина является частью типа:

```go
[3]int
[4]int
```

Это разные типы.

Slice — динамическое представление над массивом. Обычно его описывают через pointer на backing array, length и capacity.

---

### Q7. Что такое `len` и `cap` у slice?

**Ответ:**

- `len` — сколько элементов сейчас доступно.
- `cap` — сколько элементов можно разместить в текущем backing array начиная с позиции slice.

---

### Q8. Что происходит при `append`, если capacity недостаточна?

**Ответ:** Go выделяет новый backing array, копирует элементы и возвращает новый slice header. Поэтому результат `append` нужно присваивать:

```go
xs = append(xs, x)
```

---

### Q9. Может ли `append` изменить исходный slice?

**Ответ:** да. Если capacity достаточна, новый элемент попадёт в существующий backing array. Поэтому два slice, использующие один backing array, могут неожиданно увидеть изменения друг друга.

---

### Q10. Что такое full slice expression?

**Ответ:**

```go
b := a[low:high:max]
```

Она позволяет явно ограничить capacity результата:

```go
b := a[:2:2]
```

Это полезно, чтобы последующий `append` не изменял общий backing array.

---

### Q11. Что такое map?

**Ответ:** map — структура данных для отображения ключа в значение:

```go
m := map[string]int{
    "go": 1,
}
```

Доступ в среднем O(1), но нельзя рассчитывать на порядок обхода.

---

### Q12. Можно ли читать из nil map?

**Ответ:** да. Получение значения из nil map безопасно и возвращает zero value.

```go
var m map[string]int
fmt.Println(m["x"]) // 0
```

Но запись вызывает panic:

```go
m["x"] = 1 // panic
```

---

### Q13. Можно ли одновременно читать и писать map из разных goroutines?

**Ответ:** обычная map не предназначена для конкурентной записи. Нужна синхронизация: mutex, channel, `sync.Map` в подходящем сценарии или другая архитектура владения данными.

---

### Q14. Когда использовать `sync.Map`?

**Ответ:** не как универсальную замену `map + mutex`. `sync.Map` имеет смысл для определённых паттернов конкурентного доступа, например когда ключи стабильны или записи и чтения имеют специфическое соотношение. Для обычного кода чаще проще `map` + `sync.RWMutex`.

---

## 2.2 Interfaces

### Q15. Что такое interface?

**Ответ:** interface описывает набор методов. Тип автоматически удовлетворяет интерфейсу, если реализует его методы. Явно писать `implements` не нужно.

```go
type Reader interface {
    Read([]byte) (int, error)
}
```

---

### Q16. В чём сила implicit interfaces?

**Ответ:** интерфейс определяется поведением, а не наследованием. Это уменьшает связанность и позволяет объявлять интерфейс рядом с потребителем.

---

### Q17. Что такое type assertion?

```go
v, ok := x.(MyType)
```

Она проверяет, содержит ли interface значение нужного типа.

Если использовать:

```go
v := x.(MyType)
```

и тип не совпадает, будет panic.

---

### Q18. Что такое type switch?

```go
switch v := x.(type) {
case int:
    fmt.Println(v)
case string:
    fmt.Println(v)
}
```

Позволяет выполнить ветку в зависимости от динамического типа значения interface.

---

### Q19. Что такое typed nil?

Классический пример:

```go
var p *MyError = nil
var err error = p

fmt.Println(err == nil) // false
```

Interface содержит динамический тип `*MyError`, поэтому сам interface не равен nil.

---

### Q20. Pointer receiver или value receiver?

**Ответ:** pointer receiver нужен, когда метод должен изменять состояние объекта, когда копирование объекта нежелательно или объект большой.

Value receiver подходит, когда тип логически value-like и копирование нормально.

---

## 2.3 Pointers

### Q21. Что такое pointer?

**Ответ:** значение, содержащее адрес другой переменной.

```go
x := 10
p := &x
fmt.Println(*p)
```

`&x` — получить адрес, `*p` — разыменовать.

---

### Q22. Есть ли в Go pointer arithmetic?

**Ответ:** обычной pointer arithmetic, как в C, нет. Низкоуровневые операции возможны через `unsafe`, но это отдельная зона ответственности.

---

## 2.4 Functions / closures

### Q23. Функция в Go может вернуть несколько значений?

Да:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}
```

Это особенно активно используется для результата + ошибки.

---

### Q24. Что такое closure?

**Ответ:** функция, которая захватывает переменные внешнего scope.

```go
func counter() func() int {
    n := 0
    return func() int {
        n++
        return n
    }
}
```

---

# 3. defer / panic / recover

### Q25. Что делает `defer`?

**Ответ:** откладывает выполнение вызова до выхода из текущей функции.

Частый сценарий:

```go
f, err := os.Open("file")
if err != nil {
    return err
}
defer f.Close()
```

---

### Q26. В каком порядке выполняются несколько defer?

LIFO — последний зарегистрированный выполняется первым.

```go
defer fmt.Println(1)
defer fmt.Println(2)
```

Результат:

```text
2
1
```

---

### Q27. Когда вычисляются аргументы defer?

При выполнении самого `defer`, а не при фактическом выполнении отложенного вызова.

```go
x := 1
defer fmt.Println(x)
x = 2
```

Вывод:

```text
1
```

---

### Q28. А что с closure?

Closure может захватить переменную:

```go
x := 1
defer func() {
    fmt.Println(x)
}()
x = 2
```

Здесь будет `2`.

---

### Q29. Что такое panic?

`panic` прекращает обычное выполнение текущего потока и начинает раскрутку stack, выполняя defer.

---

### Q30. Что делает recover?

`recover()` позволяет перехватить panic, но практически полезен только внутри deferred function во время panic unwinding.

---

### Q31. Когда не стоит использовать panic?

Не стоит использовать panic для обычных ожидаемых ошибок: ошибка сети, неправильный input пользователя, отсутствие записи в БД и т.д.

---

# 4. Concurrency — самая важная часть Go-интервью

## Q32. Что такое goroutine?

**Ответ:** лёгкая единица выполнения, управляемая Go runtime. Она дешевле системного thread и может динамически использовать runtime scheduler.

```go
go doWork()
```

---

## Q33. Goroutine и OS thread — одно и то же?

Нет.

Грубо:

- goroutine — логическая единица выполнения Go;
- OS thread — поток операционной системы;
- Go runtime распределяет goroutines по threads.

---

## Q34. Что такое channel?

**Ответ:** типизированный механизм коммуникации между goroutines.

```go
ch := make(chan int)

go func() {
    ch <- 42
}()

x := <-ch
```

---

## Q35. Buffered и unbuffered channel?

Unbuffered:

```go
make(chan int)
```

Передача синхронизируется с получением.

Buffered:

```go
make(chan int, 10)
```

Может хранить до 10 значений без немедленного receiver, пока buffer не заполнен.

---

## Q36. Что произойдёт при отправке в заполненный buffered channel?

Отправитель заблокируется до появления места или пока канал не будет закрыт/операция не будет прервана другим механизмом.

---

## Q37. Что произойдёт при чтении из пустого channel?

Goroutine блокируется до появления значения или закрытия канала.

---

## Q38. Что происходит при чтении из закрытого channel?

Получение возвращает zero value. Чтобы отличить её от настоящего значения:

```go
v, ok := <-ch
```

После закрытия и опустошения `ok == false`.

---

## Q39. Что будет при отправке в закрытый channel?

**Ответ:** panic.

---

## Q40. Кто должен закрывать channel?

Обычно тот, кто владеет отправкой и знает, что новых значений больше не будет. Не стоит закрывать канал «на всякий случай» с другой стороны.

---

## Q41. Можно ли закрыть channel дважды?

Нет. Повторный `close` вызывает panic.

---

## Q42. Что такое select?

`select` позволяет ждать несколько channel operations:

```go
select {
case v := <-ch:
    fmt.Println(v)
case <-ctx.Done():
    return ctx.Err()
}
```

Если готовы несколько cases, выбирается один из готовых cases псевдослучайным образом.

---

## Q43. Что делает `select { default: }`?

Создаёт non-blocking проверку.

---

## Q44. Как сделать timeout для операции?

Обычно через `context` или timer:

```go
ctx, cancel := context.WithTimeout(ctx, time.Second)
defer cancel()
```

---

## Q45. Что такое data race?

Это конкурентный доступ к одной памяти, при котором хотя бы одна операция является записью, и доступы не синхронизированы.

Проверять:

```bash
go test -race ./...
```

---

## Q46. Race condition и data race — одно и то же?

Нет.

**Data race** — конкретное нарушение конкурентного доступа к памяти.

**Race condition** — более широкая логическая ситуация, когда результат зависит от порядка выполнения конкурентных операций.

---

## Q47. Mutex или channel?

Используй mutex, когда защищаешь состояние/инвариант.

Используй channels, когда строишь коммуникацию/передачу ownership или pipeline.

Не нужно искусственно заменять каждый mutex каналом.

---

## Q48. Mutex и RWMutex?

`sync.Mutex` — один lock.

`sync.RWMutex` позволяет нескольким readers одновременно держать read lock, но writer требует эксклюзивный доступ.

`RWMutex` не автоматически быстрее Mutex: всё зависит от профиля нагрузки.

---

## Q49. Что такое WaitGroup?

Механизм ожидания завершения группы goroutines.

Современный безопасный паттерн:

```go
var wg sync.WaitGroup

wg.Add(1)
go func() {
    defer wg.Done()
    work()
}()

wg.Wait()
```

Важно корректно управлять количеством `Add` и `Done`.

---

## Q50. Что такое sync.Once?

Гарантирует однократное выполнение функции даже при конкурентном вызове:

```go
var once sync.Once

once.Do(func() {
    initialize()
})
```

---

## Q51. Что такое atomic?

Атомарные операции позволяют безопасно выполнять определённые операции над общими значениями без обычного mutex.

Подходят для простых счётчиков, флагов и некоторых lock-free структур.

---

## Q52. Что такое deadlock?

Состояние, при котором goroutines ждут друг друга и никто не может продолжить.

Классический пример:

```go
ch := make(chan int)
ch <- 1
```

Если нет receiver, текущая goroutine блокируется.

---

## Q53. Что такое goroutine leak?

Goroutine продолжает жить, хотя логически больше не нужна, потому что застряла на channel receive/send, mutex, I/O или другой операции.

---

## Q54. Как предотвращать goroutine leaks?

- context cancellation
- timeouts
- ownership каналов
- корректное закрытие/завершение pipeline
- не запускать goroutine без плана её завершения
- тестировать lifecycle

---

## Q55. Что такое worker pool?

Набор worker goroutines, которые получают задачи из очереди:

```text
producer
   |
   v
 jobs channel
   |
   +--> worker 1
   +--> worker 2
   +--> worker 3
```

Полезен для ограничения concurrency и защиты внешнего ресурса.

---

## Q56. Что такое fan-out?

Один поток задач распределяет работу между несколькими workers.

---

## Q57. Что такое fan-in?

Несколько источников объединяются в один channel/поток.

---

## Q58. Что такое pipeline?

Последовательность стадий обработки:

```text
input -> stage1 -> stage2 -> stage3 -> output
```

Каждая стадия может быть отдельной goroutine и общаться через channels.

---

# 5. Context

## Q59. Зачем нужен context?

Для передачи:

- cancellation
- deadlines
- request-scoped values

между слоями и goroutines.

---

## Q60. Что такое `context.Context`?

Интерфейс, который позволяет узнать deadline/cancellation и получить значения request scope.

---

## Q61. Почему нельзя передавать context как глобальную переменную?

Context должен быть связан с конкретной операцией/request lifecycle. Глобальный context ломает cancellation и усложняет управление жизненным циклом.

---

## Q62. Где обычно передают context?

Как первый аргумент функции:

```go
func GetUser(ctx context.Context, id int) (*User, error)
```

---

## Q63. Нужно ли передавать context в struct?

Обычно нет. Context — параметр операции, а не постоянное состояние объекта.

---

# 6. Memory model / runtime

## Q64. Что такое happens-before?

Это отношение, определяющее, когда операции в одной goroutine гарантированно становятся наблюдаемыми другой goroutine через правила синхронизации.

---

## Q65. Как синхронизировать доступ к общей памяти?

Например:

- mutex
- channel
- atomic
- другие primitives из `sync`

Официальная модель Go прямо рекомендует синхронизировать данные, которые одновременно модифицируются несколькими goroutines.

---

## Q66. Что такое escape analysis?

Компилятор анализирует, где должны жить значения. Если значение должно пережить текущий stack frame или доступно из места, несовместимого со stack allocation, оно может «escape» в heap.

Проверка:

```bash
go build -gcflags="-m"
```

---

## Q67. Stack или heap — что быстрее?

Нельзя сводить ответ к «stack всегда быстрее». Stack allocation обычно дешевле, а heap allocation создаёт дополнительную работу для allocator/GC. Но важен весь generated code и lifetime объекта.

---

## Q68. Что делает GC?

Garbage collector автоматически освобождает недостижимые объекты в heap.

---

## Q69. Почему большое количество маленьких allocations может быть проблемой?

Потому что увеличивается работа allocator и garbage collector, растёт CPU overhead и давление на память.

---

# 7. Go 1.26 — что знать на актуальном интервью

## Q70. Какая актуальная major/minor ветка Go?

На август 2026 — **Go 1.26.x**.

---

## Q71. Что важного появилось в Go 1.26?

Нужно знать хотя бы на уровне awareness:

- `new` теперь принимает выражение, задающее начальное значение;
- generic-типы получили расширение, позволяющее ссылаться на себя в списке type parameters;
- Green Tea GC включён по умолчанию;
- снижены некоторые cgo overhead;
- улучшены возможности stack allocation;
- `go fix` получил новый анализирующий/modernizing механизм;
- появились новые/экспериментальные возможности runtime и SIMD.

Не нужно зубрить release notes, если позиция не связана с compiler/runtime.

---

# 8. Errors

## Q72. Почему Go использует ошибки как значения?

Потому что ошибка является частью обычного результата функции и явно обрабатывается вызывающим кодом.

```go
value, err := doSomething()
if err != nil {
    return err
}
```

---

## Q73. Что делает `%w`?

Позволяет оборачивать error:

```go
return fmt.Errorf("get user: %w", err)
```

После этого можно использовать `errors.Is`/`errors.As`.

---

## Q74. `errors.Is` и `errors.As`?

`errors.Is` проверяет соответствие конкретной ошибке/цепочке.

`errors.As` ищет ошибку определённого типа.

---

## Q75. Почему нельзя просто сравнивать ошибки через `==`?

Иногда можно для конкретных sentinel errors, но при wrapping это недостаточно.

Используй `errors.Is` для проверки семантического соответствия.

---

# 9. Interfaces + generics

## Q76. Generics или interfaces?

Interfaces удобны для поведения и dynamic dispatch.

Generics удобны, когда алгоритм должен работать с разными типами, сохраняя compile-time type information.

Пример:

```go
func Map[T any](xs []T) []T {
    return xs
}
```

---

## Q77. Что такое type constraint?

Ограничение, определяющее допустимые типы для type parameter.

---

## Q78. Нужно ли всё переписывать на generics?

Нет. Generics — инструмент, а не цель. В Go обычно ценится простота.

---

# 10. Testing

## Q79. Как написать unit test?

```go
func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

---

## Q80. Что такое table-driven test?

Набор входов/ожидаемых результатов:

```go
tests := []struct{
    name string
    input int
    want int
}{
    {"zero", 0, 0},
    {"one", 1, 1},
}
```

Потом каждый кейс запускается через `t.Run`.

---

## Q81. Зачем `t.Run`?

Для именованных subtests и изоляции отдельных сценариев.

---

## Q82. Что делает `go test -race`?

Запускает race detector и помогает находить data races.

---

## Q83. Что такое benchmark?

Специальный тест для измерения производительности:

```go
func BenchmarkFoo(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Foo()
    }
}
```

---

## Q84. Почему нельзя делать выводы о производительности по одному benchmark?

Потому что результат зависит от среды, compiler optimizations, CPU, cache, GC, нагрузки и методологии. Нужно сравнивать controlled benchmarks и смотреть allocations/CPU profiles.

---

## Q85. Что такое fuzz testing?

Автоматический поиск неожиданных входных данных, которые приводят к ошибкам/нарушению свойств программы.

---

# 11. HTTP / Backend

## Q86. Как создать HTTP server в Go?

Минимально:

```go
mux := http.NewServeMux()

mux.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
})

server := &http.Server{
    Addr:    ":8080",
    Handler: mux,
}

log.Fatal(server.ListenAndServe())
```

---

## Q87. Почему важно использовать timeouts?

Без timeout сетевой запрос может зависнуть надолго и занять resource.

Нужно думать как минимум о:

- request context
- server read/write/idle timeouts
- client timeout
- DB timeout
- downstream timeout

---

## Q88. Что такое middleware?

Функция, оборачивающая handler для cross-cutting logic:

```text
request
  |
logging
  |
auth
  |
metrics
  |
handler
```

---

## Q89. Что такое graceful shutdown?

Сервер перестаёт принимать новую работу, ждёт завершения текущих запросов в допустимых пределах и закрывает ресурсы.

---

## Q90. Что делать при shutdown?

Типичный порядок:

1. получить SIGTERM/SIGINT;
2. отменить общий context;
3. остановить приём новых запросов;
4. дождаться текущих запросов;
5. закрыть DB/queues/clients;
6. завершить процесс.

---

# 12. SQL / PostgreSQL

## Q91. Что такое transaction?

Группа операций, выполняемая как единое логическое изменение данных.

---

## Q92. Что такое ACID?

- Atomicity — либо всё, либо ничего.
- Consistency — сохраняются ограничения.
- Isolation — параллельные операции не нарушают заданную модель изоляции.
- Durability — подтверждённые данные сохраняются.

---

## Q93. Что такое connection pool?

Пул переиспользуемых соединений к БД.

Без него приложение может создавать слишком много соединений и перегружать БД.

---

## Q94. Почему нельзя открывать новое DB connection на каждый request?

Это дорого и может быстро исчерпать лимит соединений PostgreSQL.

---

## Q95. Что такое N+1 query problem?

Например:

```text
1 query -> получить 100 users
100 queries -> получить данные каждого user
```

Всего 101 query вместо оптимизированного набора запросов.

---

# 13. Redis / Kafka

## Q96. Когда Redis полезен?

- cache
- counters
- distributed locks (с осторожностью)
- rate limiting
- ephemeral state
- pub/sub
- очереди/streams в подходящих архитектурах

---

## Q97. Почему Redis cache не является заменой PostgreSQL?

Redis и PostgreSQL решают разные задачи. Cache может быть потерян/истечь, а основное состояние должно иметь подходящее persistent storage.

---

## Q98. Что такое Kafka?

Распределённая система event streaming. Producers записывают сообщения в topics/partitions, consumers читают их, обычно группами consumer groups.

---

## Q99. Что такое partition?

Логически отдельный упорядоченный журнал внутри topic. Порядок гарантируется внутри partition, а не глобально по всему topic.

---

## Q100. Что такое consumer group?

Группа consumers, совместно обрабатывающая partitions topic. Обычно одна partition в рамках одной consumer group обрабатывается одним consumer одновременно.

---

# 14. gRPC

## Q101. Что такое gRPC?

RPC framework, часто использующий HTTP/2 и Protocol Buffers.

---

## Q102. REST или gRPC?

REST удобен для публичных HTTP API и широкого ecosystem.

gRPC особенно удобен для service-to-service коммуникации, строгих контрактов, streaming и высокой эффективности.

---

# 15. Observability

## Q103. Три основных столпа observability?

- logs
- metrics
- traces

---

## Q104. Что измерять в backend?

Минимум:

- request rate
- latency
- error rate
- saturation
- DB latency
- external dependency latency
- queue lag
- goroutines
- memory
- CPU

---

## Q105. Что такое pprof?

Набор инструментов Go для profiling CPU, memory, goroutines, mutex/blocking и других характеристик runtime/application.

---

# 16. Go internals — GOOD TO KNOW

## Q106. Что такое G/M/P?

Упрощённая модель runtime scheduler:

- G — goroutine;
- M — OS thread;
- P — processor/context, необходимый M для выполнения Go code.

---

## Q107. Зачем нужен P?

Он содержит состояние, необходимое runtime для выполнения goroutines, включая локальную run queue.

---

## Q108. Что такое work stealing?

Если у processor заканчиваются свои runnable goroutines, scheduler может брать работу у другого processor.

---

## Q109. Что такое preemption?

Runtime может остановить выполняющуюся goroutine, чтобы другие goroutines получили возможность выполняться.

---

## Q110. Что такое GOMAXPROCS?

Ограничивает количество processors, одновременно выполняющих Go code. Значение и поведение зависят от версии Go и среды выполнения; в современных версиях Go runtime также учитывает container CPU limits.

---

# 17. Map internals — GOOD TO KNOW

## Q111. Почему map не гарантирует порядок?

Порядок обхода не является контрактом языка. Нельзя строить бизнес-логику на конкретном порядке `range` по map.

---

## Q112. Как приблизительно работает hash map?

Ключ проходит через hash function, после чего runtime ищет соответствующий bucket/структуру хранения и сравнивает ключи.

Не нужно на Junior собеседовании воспроизводить поля runtime structure наизусть. Важно понимать hash → bucket → поиск.

---

# 18. Slices — типовые ловушки

## Q113. Что выведет?

```go
a := []int{1, 2, 3}
b := a[:2]
b[0] = 100

fmt.Println(a)
```

**Ответ:**

```text
[100 2 3]
```

Потому что `a` и `b` используют один backing array.

---

## Q114. Как избежать общего backing array?

Копировать данные:

```go
b := append([]int(nil), a[:2]...)
```

или использовать `slices.Clone` в современных версиях Go.

---

# 19. Interface trap

## Q115. Что выведет?

```go
var p *int = nil
var x any = p

fmt.Println(x == nil)
```

**Ответ:**

```text
false
```

Потому что interface содержит dynamic type `*int` и nil value.

---

# 20. defer trap

## Q116. Что выведет?

```go
func f() {
    x := 1
    defer fmt.Println(x)
    x = 2
}
```

**Ответ:** `1`.

---

## Q117. Что выведет?

```go
func f() {
    x := 1

    defer func() {
        fmt.Println(x)
    }()

    x = 2
}
```

**Ответ:** `2`.

---

# 21. Channel traps

## Q118. Что произойдёт?

```go
ch := make(chan int)
ch <- 1
```

**Ответ:** deadlock, если нет другой goroutine, которая принимает значение.

---

## Q119. Что произойдёт?

```go
ch := make(chan int)
close(ch)

fmt.Println(<-ch)
```

**Ответ:** zero value типа `int`, то есть `0`.

---

## Q120. Что произойдёт?

```go
ch := make(chan int)
close(ch)
ch <- 1
```

**Ответ:** panic.

---

# 22. Coding tasks — MUST PRACTICE

## Задача 1. Пересечение двух slices

Требования:

- O(n+m) в среднем;
- учитывать дубликаты или явно проговорить семантику;
- написать тесты.

---

## Задача 2. Удалить дубликаты из slice

Нужно объяснить:

- complexity;
- memory;
- сохраняется ли порядок.

---

## Задача 3. Reverse linked list

Проверяется:

- pointers;
- loops;
- edge cases.

---

## Задача 4. LRU cache

Ожидаемый подход:

```text
map[key]*Node
        +
doubly linked list
```

Complexity:

- get — O(1)
- put — O(1)

---

## Задача 5. Worker pool

Реализовать:

```text
jobs -> N workers -> results
```

Обязательно продумать:

- закрытие jobs;
- завершение workers;
- cancellation;
- error propagation;
- отсутствие goroutine leaks.

---

## Задача 6. Fan-in

Объединить несколько channels:

```go
func merge[T any](channels ...<-chan T) <-chan T
```

Нужно корректно закрыть output после завершения всех inputs.

---

## Задача 7. Pipeline

Сделать:

```text
numbers -> filter -> square -> output
```

Добавить context cancellation.

---

## Задача 8. Rate limiter

Реализовать ограничение количества операций в секунду.

Варианты:

- ticker;
- token bucket;
- semaphore.

---

## Задача 9. Concurrent counter

Сделать безопасный counter.

Сравнить:

- Mutex;
- atomic;
- channel ownership.

---

## Задача 10. HTTP client с retry

Требования:

- timeout;
- context;
- exponential backoff;
- максимальное число retry;
- не повторять бездумно non-idempotent операции.

---

# 23. System Design — Go Backend

## MUST KNOW

Уметь спроектировать:

### 1. URL shortener

```text
Client
  |
API
  |
Service
  |
Redis ---- PostgreSQL
```

Обсудить:

- ID generation;
- collision;
- cache;
- TTL;
- read/write ratio;
- analytics.

### 2. Chat service

Обсудить:

- WebSocket;
- connection manager;
- Redis Pub/Sub;
- persistence;
- reconnect;
- ordering;
- delivery semantics;
- horizontal scaling.

### 3. Notification service

```text
API
 |
Queue
 |
Workers
 |
Email/SMS/Push
```

Обсудить:

- retry;
- DLQ;
- idempotency;
- backpressure;
- rate limits.

### 4. File upload service

Обсудить:

- object storage;
- metadata DB;
- multipart upload;
- checksum;
- virus scanning;
- async processing.

---

# 24. Architecture questions

## Q121. Как сделать Go-сервис устойчивым?

Ответ должен включать:

- timeouts;
- cancellation;
- retries с backoff;
- circuit breaker;
- bounded concurrency;
- connection pooling;
- health checks;
- metrics;
- logs;
- tracing;
- graceful shutdown.

---

## Q122. Почему retry может быть опасным?

Потому что retry увеличивает нагрузку и может создать retry storm.

Особенно опасно повторять операции, которые не являются идемпотентными.

---

## Q123. Что такое idempotency?

Повтор одного и того же запроса приводит к тому же логическому состоянию.

Для платежей/создания ресурсов часто используют idempotency key.

---

## Q124. Что такое backpressure?

Механизм, заставляющий producer учитывать скорость consumer.

В Go это можно реализовать через:

- bounded channels;
- worker pool;
- semaphore;
- rate limiter;
- queue limits.

---

# 25. Production checklist

Перед production Go-сервисом проверь:

- [ ] context используется корректно
- [ ] нет goroutine leaks
- [ ] нет неограниченного создания goroutines
- [ ] есть timeouts
- [ ] есть graceful shutdown
- [ ] DB pool ограничен
- [ ] запросы к БД индексированы
- [ ] есть structured logging
- [ ] есть metrics
- [ ] есть tracing при необходимости
- [ ] `go test ./...`
- [ ] `go test -race ./...`
- [ ] `go vet ./...`
- [ ] benchmarks для критических мест
- [ ] profiling для реальных bottleneck
- [ ] зависимости обновляются контролируемо
- [ ] секреты не лежат в коде
- [ ] Docker image минимизирован
- [ ] health/readiness endpoints
- [ ] лимиты CPU/memory
- [ ] обработка SIGTERM

---

# 26. Частые ошибки кандидатов

1. Говорят «goroutine = thread».
2. Говорят «channel всегда лучше mutex».
3. Не знают, что запись в обычную map конкурентно опасна.
4. Не знают typed nil.
5. Не знают порядок defer.
6. Не понимают slice/backing array.
7. Не умеют объяснить context.
8. Не знают race detector.
9. Не умеют написать worker pool.
10. Не думают о graceful shutdown.
11. Делают retry без timeout/backoff.
12. Не знают `errors.Is`/`errors.As`.
13. Не умеют читать чужой concurrent code.
14. Путают concurrency и parallelism.
15. Считают любой benchmark доказательством производительности.
16. Не умеют объяснить, зачем нужен connection pool.
17. Не знают, что Kafka сохраняет порядок только в рамках partition.
18. Не думают о cancellation.
19. Запускают goroutine и не знают, кто её остановит.
20. Не умеют объяснить собственный проект.

---

# 27. Что отвечать, если не знаешь internals

Хороший ответ:

> «На уровне API я понимаю механизм и могу объяснить поведение. Внутреннюю реализацию конкретной версии runtime я не помню наизусть, но знаю, где её проверить и какие инварианты она должна соблюдать.»

Плохой ответ:

> «Ну Go сам это как-то делает.»

---

# 28. Must Know vs Good to Know

| Тема | Уровень |
|---|---|
| Variables/types | 🔴 MUST |
| Slice | 🔴 MUST |
| Map | 🔴 MUST |
| Struct | 🔴 MUST |
| Interface | 🔴 MUST |
| Pointer | 🔴 MUST |
| Errors | 🔴 MUST |
| defer | 🔴 MUST |
| panic/recover | 🔴 MUST |
| Goroutines | 🔴 MUST |
| Channels | 🔴 MUST |
| select | 🔴 MUST |
| Mutex | 🔴 MUST |
| WaitGroup | 🔴 MUST |
| Context | 🔴 MUST |
| Race detector | 🔴 MUST |
| Testing | 🔴 MUST |
| HTTP | 🔴 MUST |
| SQL | 🔴 MUST |
| Docker | 🔴 MUST |
| Graceful shutdown | 🔴 MUST |
| Redis | 🟡 GOOD |
| Kafka | 🟡 GOOD |
| gRPC | 🟡 GOOD |
| pprof | 🟡 GOOD |
| GC | 🟡 GOOD |
| escape analysis | 🟡 GOOD |
| G/M/P | 🟡 GOOD |
| scheduler internals | 🟣 DEEP |
| GC internals | 🟣 DEEP |
| compiler SSA | 🟣 DEEP |
| runtime source code | 🟣 DEEP |
| `unsafe` internals | 🟣 DEEP |

---

# 29. Оптимальный порядок подготовки

## День 1

- syntax
- types
- arrays
- slices
- maps
- structs

## День 2

- pointers
- methods
- interfaces
- type assertions
- generics

## День 3

- errors
- defer
- panic
- recover

## День 4

- goroutines
- channels
- select

## День 5

- mutex
- WaitGroup
- atomic
- race detector

## День 6

- context
- worker pool
- fan-in/fan-out
- pipeline

## День 7

- testing
- benchmarks
- fuzzing

## День 8

- HTTP
- REST
- middleware
- graceful shutdown

## День 9

- PostgreSQL
- transactions
- indexes
- connection pools

## День 10

- Redis
- Kafka
- gRPC

## День 11

- runtime
- GC
- escape analysis
- scheduler

## День 12

- profiling
- pprof
- observability

## День 13

- system design

## День 14

- mock interview
- coding tasks
- code review

---

# 30. Лучшие актуальные ресурсы

## 30.1 Interview — основной набор

### 1. goavengers/go-interview

Большая русскоязычная коллекция вопросов для Backend/Golang. Особенно полезна для:

- базовых вопросов;
- maps;
- slices;
- concurrency;
- mutex;
- channels;
- задач на собеседовании.

Репозиторий: https://github.com/goavengers/go-interview

### 2. defer-panic/awesome-go-interview

Кураторский список ресурсов именно для Go-интервью:

- Go;
- backend;
- architecture;
- system design;
- CS;
- coding challenges.

Репозиторий: https://github.com/defer-panic/awesome-go-interview

### 3. Devinterview-io/golang-interview-questions

Современная подборка из 100 вопросов, ориентированная на интервью.

Репозиторий: https://github.com/Devinterview-io/golang-interview-questions

### 4. yakomisar/golang-questions

Русскоязычные вопросы по:

- syntax;
- slices;
- maps;
- pointers;
- goroutines;
- channels;
- strings;
- corner cases.

Репозиторий: https://github.com/yakomisar/golang-questions

### 5. rixtrayker/golang-interview

Большой формат книги с вопросами, ответами, примерами и use cases.

Репозиторий: https://github.com/rixtrayker/golang-interview

---

# 31. Изучение самого Go

## 1. Official Go documentation

Обязательно:

- Specification
- Effective Go
- FAQ
- Memory Model
- Diagnostics
- GC guide
- Testing
- Fuzzing
- PGO

https://go.dev/doc/

## 2. Go Specification

Для глубокого понимания языка:

https://go.dev/ref/spec

## 3. Go Memory Model

Для concurrency:

https://go.dev/ref/mem

## 4. Go release notes

Следить за изменениями Go:

https://go.dev/doc/devel/release

---

# 32. Roadmap

## roadmap.sh Go Developer

Полезный современный roadmap:

https://roadmap.sh/golang

Он покрывает:

- Go language;
- backend;
- databases;
- testing;
- tooling;
- performance;
- production.

Проекты:

https://roadmap.sh/golang/projects

---

# 33. Internals

## teh-cmc/go-internals

Глубокое изучение внутренних механизмов Go:

- compiler;
- runtime;
- scheduler;
- GC;
- memory;
- interfaces;
- maps;
- slices.

https://github.com/teh-cmc/go-internals

## tmrts/go-patterns

Практические Go design/concurrency patterns:

- worker;
- bounded parallelism;
- producer/consumer;
- semaphore;
- pipeline;
- parallelism.

https://github.com/tmrts/go-patterns

---

# 34. Testing

## quii/learn-go-with-tests

Один из лучших бесплатных практических ресурсов:

- Go fundamentals;
- TDD;
- slices;
- maps;
- structs;
- interfaces;
- errors;
- HTTP;
- concurrency;
- testing.

https://github.com/quii/learn-go-with-tests

---

# 35. Style / production code

## Uber Go Style Guide

Очень полезно для code review и production interview:

- interfaces;
- receivers;
- errors;
- defer;
- channels;
- atomics;
- globals;
- performance;
- naming;
- test tables;
- functional options.

https://github.com/uber-go/guide

---

# 36. Books / long-form resources

## GoBooks

Большая подборка книг:

https://github.com/dariubs/GoBooks

Особенно полезны:

1. Learning Go, 2nd Edition.
2. 100 Go Mistakes and How to Avoid Them.
3. Let's Go.
4. Let's Go Further.
5. The Go Programming Language.
6. Learn Go With Tests.
7. High Performance Go.
8. Go internals resources.

---

# 37. Что реально нужно знать Junior Go Backend

Если времени мало, закрой это на 100%:

### Language

- [ ] slices
- [ ] maps
- [ ] structs
- [ ] interfaces
- [ ] pointers
- [ ] methods
- [ ] errors
- [ ] defer
- [ ] closures
- [ ] generics basics

### Concurrency

- [ ] goroutines
- [ ] channels
- [ ] buffered/unbuffered
- [ ] select
- [ ] mutex
- [ ] WaitGroup
- [ ] atomic
- [ ] race detector
- [ ] context
- [ ] worker pool
- [ ] goroutine leaks

### Backend

- [ ] HTTP
- [ ] REST
- [ ] JSON
- [ ] middleware
- [ ] timeouts
- [ ] graceful shutdown
- [ ] PostgreSQL
- [ ] transactions
- [ ] indexes
- [ ] connection pool
- [ ] Redis basics
- [ ] Docker

### Engineering

- [ ] unit tests
- [ ] integration tests
- [ ] benchmarks
- [ ] logging
- [ ] metrics
- [ ] Git
- [ ] CI/CD basics

---

# 38. Что нужно для сильного Junior / Junior+

Дополнительно:

- [ ] gRPC
- [ ] Kafka
- [ ] Redis advanced
- [ ] pprof
- [ ] profiling
- [ ] escape analysis
- [ ] GC basics
- [ ] scheduler G/M/P
- [ ] distributed systems basics
- [ ] idempotency
- [ ] retry/backoff
- [ ] rate limiting
- [ ] circuit breaker
- [ ] OpenTelemetry
- [ ] Kubernetes basics
- [ ] system design fundamentals

---

# 39. Финальный self-test

Ты готов к Go interview, если можешь без подсказок:

1. Объяснить slice и backing array.
2. Объяснить map и concurrent access.
3. Объяснить interface и typed nil.
4. Объяснить pointer/value receiver.
5. Объяснить defer LIFO.
6. Объяснить panic/recover.
7. Объяснить goroutine.
8. Объяснить channel.
9. Объяснить close channel.
10. Объяснить select.
11. Написать worker pool.
12. Написать fan-in.
13. Написать pipeline.
14. Объяснить mutex vs channel.
15. Объяснить data race.
16. Запустить race detector.
17. Объяснить context cancellation.
18. Написать graceful shutdown.
19. Написать HTTP handler.
20. Написать unit tests.
21. Написать benchmark.
22. Объяснить SQL transaction.
23. Объяснить connection pool.
24. Объяснить Redis cache.
25. Объяснить Kafka partition.
26. Объяснить gRPC.
27. Объяснить pprof.
28. Объяснить GC.
29. Объяснить escape analysis.
30. Нарисовать архитектуру production Go service.

---

# 40. Главное

Для Go-собеседования не нужно превращаться в энциклопедию runtime.

Сильный кандидат должен показать три уровня мышления:

### Уровень 1 — «Я знаю Go»

```text
syntax
types
interfaces
slices
maps
errors
defer
generics
```

### Уровень 2 — «Я умею писать конкурентный backend»

```text
goroutines
channels
mutex
context
timeouts
testing
HTTP
SQL
Redis
Kafka
```

### Уровень 3 — «Я понимаю production»

```text
cancellation
backpressure
idempotency
retries
observability
profiling
graceful shutdown
scaling
system design
```

Именно переход от первого уровня к третьему обычно сильнее всего влияет на впечатление от Go Backend кандидата.

---

# 41. Источники, использованные при составлении

- Go Specification — официальная спецификация языка.
- Go Memory Model — официальная модель памяти.
- Go Documentation — официальная документация.
- Go 1.26 Release Notes — актуальные изменения языка/runtime/toolchain.
- goavengers/go-interview — русскоязычная коллекция Go/backend interview вопросов.
- defer-panic/awesome-go-interview — curated Go interview resources.
- Devinterview-io/golang-interview-questions — современная подборка interview questions.
- yakomisar/golang-questions — русскоязычные вопросы и corner cases.
- rixtrayker/golang-interview — книга по Go interview.
- roadmap.sh/golang — современный Go Developer Roadmap.
- quii/learn-go-with-tests — практическое обучение Go через тесты.
- uber-go/guide — production-oriented Go Style Guide.
- tmrts/go-patterns — Go patterns и concurrency patterns.
- dariubs/GoBooks — curated Go books/resources.
- teh-cmc/go-internals — Go internals.

---

# 42. Важное замечание по актуальности

Go быстро развивается. На август 2026 актуальная major/minor ветка — **Go 1.26.x**, поэтому старые интервью-репозитории полезно использовать как базу вопросов, но спорные детали runtime, compiler, GC и стандартной библиотеки нужно сверять с официальной документацией текущей версии.

Старый вопрос может быть полезным для проверки понимания, даже если конкретная реализация runtime изменилась.

**Правило:** поведение языка → Specification; memory/concurrency guarantees → Memory Model; версия/нововведения → Release Notes; style → актуальные production guides.

