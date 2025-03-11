Go предоставляет **три механизма** обработки ошибок:
1. **Обычные ошибки (`error`)** — стандартный способ работы с ошибками.
2. **Паники (`panic`)** — для критических ситуаций, когда программу нельзя продолжать.
3. **Восстановление (`recover`)** — позволяет "спастись" после паники.

---
## Обычные ошибки (`error`)
Go **не использует исключения** (`try-catch`). Вместо этого большинство функций возвращает `error`.

```go
package main

import (
	"errors"
	"fmt"
)

// Функция возвращает ошибку
func divide(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("деление на ноль")
	}
	return a / b, nil
}

func main() {
	result, err := divide(10, 0)
	if err != nil {
		fmt.Println("Ошибка:", err)
		return
	}
	fmt.Println("Результат:", result)
}
```

Вывод:
```
Ошибка: деление на ноль
```

**Как работает?**
- Если ошибки нет → возвращаем `nil` в качестве `error`.
- Если есть ошибка → возвращаем `errors.New("текст ошибки")`.
- В `main()` проверяем `err != nil` перед использованием результата.

## Создание ошибок (`errors` и `fmt.Errorf`)

Go предоставляет два способа создания ошибок:
1. `errors.New("текст ошибки")`
2. `fmt.Errorf("описание: %w", err)` (оборачивает ошибку)

Пример с `fmt.Errorf()`:
```go
package main

import (
	"errors"
	"fmt"
)

// Функция с ошибкой
func openFile(filename string) error {
	return fmt.Errorf("не удалось открыть файл %s: %w", filename, errors.New("файл не найден"))
}

func main() {
	err := openFile("data.txt")
	if err != nil {
		fmt.Println("Ошибка:", err)
	}
}

```

Вывод:
```
Ошибка: не удалось открыть файл data.txt: файл не найден
```

Здесь `%w` позволяет **оборачивать ошибки**, сохраняя цепочку.

## Проверка типа ошибки (`errors.Is` и `errors.As`)

### `errors.Is()` — проверяет, является ли ошибка конкретной

```go
var ErrNotFound = errors.New("файл не найден")

func openFile(filename string) error {
	return fmt.Errorf("ошибка: %w", ErrNotFound)
}

func main() {
	err := openFile("data.txt")
	if errors.Is(err, ErrNotFound) {
		fmt.Println("Файл отсутствует")
	}
}
```

Вывод:

```
Файл отсутствует
```

### `errors.As()` — извлекает конкретный тип ошибки

Если используем **кастомные ошибки**:
```go
type MyError struct {
	Code int
	Msg  string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("код %d: %s", e.Code, e.Msg)
}

func process() error {
	return &MyError{Code: 404, Msg: "ресурс не найден"}
}

func main() {
	err := process()
	var myErr *MyError
	if errors.As(err, &myErr) {
		fmt.Println("Ошибка кода:", myErr.Code)
	}
}
```

Вывод:

```
Ошибка кода: 404
```

## Паника (`panic`)

Используется, если программа **не может продолжать работу** (например, критическая ошибка).

### ❌ Плохой пример (неправильное использование `panic`)
```go
func divide(a, b int) int {
	if b == 0 {
		panic("деление на ноль") // ❌ НЕ ДЕЛАЙ ТАК
	}
	return a / b
}
```
Лучше возвращать `error`, а не `panic`, если можно обработать ошибку.

### ✅ Когда использовать `panic`?
- Ошибки **внутри `main()`**, когда программа должна аварийно завершиться.
- Ошибки **при запуске сервера**, если нет смысла продолжать.
- Нарушение **внутренних инвариантов** (например, при использовании `mustX()` функций).

```go
package main

import "fmt"

func main() {
	fmt.Println("До паники")
	panic("Что-то пошло не так")
	fmt.Println("После паники") // НЕ выполнится
}
```
## Восстановление после паники (`recover`)
Если `panic` произошёл в `goroutine`, её можно поймать с помощью `recover()`.

```go
package main

import "fmt"

func safeFunction() {
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("Восстановлено после паники:", r)
		}
	}()

	panic("Критическая ошибка") // Это вызовет recover
}

func main() {
	fmt.Println("До вызова safeFunction")
	safeFunction()
	fmt.Println("После вызова safeFunction") // Выполнится
}
```

Программа **не завершилась аварийно**, потому что `recover()` поймал `panic`.

## Ошибки vs Паники

|**Ошибка (`error`)**|**Паника (`panic`)**|
|---|---|
|Обычное поведение|Критическая ошибка|
|Можно обработать|Прерывает выполнение|
|Используется в функциях|Используется при катастрофических сбоях|
|`return err`|`panic("message")`|

**Правило:** Используй `error` для **ожидаемых ошибок**, `panic` — только если **нет смысла продолжать работу**.

## Пример обработки ошибок и паники в HTTP-сервере

```go
package main

import (
	"fmt"
	"net/http"
)

// Middleware для перехвата паники
func recoveryMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		defer func() {
			if err := recover(); err != nil {
				http.Error(w, "Внутренняя ошибка сервера", http.StatusInternalServerError)
				fmt.Println("Паника перехвачена:", err)
			}
		}()
		next.ServeHTTP(w, r)
	})
}

func handler(w http.ResponseWriter, r *http.Request) {
	panic("Что-то сломалось!") // Паника в обработчике
}

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", handler)

	server := http.Server{
		Addr:    ":8080",
		Handler: recoveryMiddleware(mux),
	}

	fmt.Println("Сервер запущен на порту 8080")
	server.ListenAndServe()
}
```

Здесь `recover()` не даёт **упасть всему серверу** из-за ошибки в одном обработчике.

## Итог
✅ **`error`** — основной механизм обработки ошибок, лучше использовать `fmt.Errorf()` и `errors.Is()`.  
✅ **`panic`** — только в крайних случаях (например, критические ошибки).  
✅ **`recover`** — ловит `panic`, полезно для восстановления работы (например, в HTTP-серверах).