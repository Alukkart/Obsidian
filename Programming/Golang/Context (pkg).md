Пакет `context` в Go используется для управления временем выполнения горутин и передачи значений между ними. Он полезен, когда нужно **отменять операции**, **задавать тайм-ауты** или **передавать данные между обработчиками**.
## Создание базового контекста
Функция `context.Background()` создаёт пустой **родительский контекст**, который обычно используется в `main()` или `init()`.

```go
package main

import (
	"context"
	"fmt"
)

func main() {
	ctx := context.Background()
	fmt.Println(ctx) // context.Background
}
```
Также есть `context.TODO()`, который используется, если пока не понятно, какой контекст нужен.

## Тайм-аут и дедлайн
Можно ограничить выполнение операции по времени.

### `context.WithTimeout()`

Создаёт контекст, который автоматически отменяется через заданное время.
```go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
	defer cancel() // Освобождаем ресурсы

	select {
	case <-time.After(3 * time.Second):
		fmt.Println("Операция завершена")
	case <-ctx.Done():
		fmt.Println("Контекст отменён:", ctx.Err()) // DeadlineExceeded
	}
}
```

Вывод через 2 секунды:

```
Контекст отменён: context deadline exceeded
```

### `context.WithDeadline()`

Работает так же, но использует **точное время завершения**.

```go
deadline := time.Now().Add(2 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
```

## Отмена (`context.WithCancel`)
Создаёт дочерний контекст, который можно **отменить вручную**.

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func worker(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("Работник остановлен:", ctx.Err())
			return
		default:
			fmt.Println("Работаю...")
			time.Sleep(500 * time.Millisecond)
		}
	}
}

func main() {
	ctx, cancel := context.WithCancel(context.Background())

	go worker(ctx)

	time.Sleep(2 * time.Second)
	cancel() // Останавливаем горутину

	time.Sleep(1 * time.Second) // Ждём, чтобы увидеть вывод
}
```

Вывод:

```
Работаю...
Работаю...
Работаю...
Работаю...
Работник остановлен: context canceled
```

## Передача значений (`context.WithValue`)
Можно передавать значения через контекст (но **использовать его как глобальный storage — плохая практика**).

```go
package main

import (
	"context"
	"fmt"
)

func process(ctx context.Context) {
	userID := ctx.Value("userID")
	fmt.Println("Обрабатываем пользователя:", userID)
}

func main() {
	ctx := context.WithValue(context.Background(), "userID", 42)
	process(ctx)
}
```

Вывод:
```
Обрабатываем пользователя: 42
```

**Важно:** `context.WithValue()` не используется для передачи больших данных — лучше передавать их как аргументы функций.

## Вложенные контексты
Можно комбинировать **отмену**, **тайм-ауты** и **значения**.

```go
ctx := context.WithValue(context.Background(), "user", "Alice")
ctx, cancel := context.WithTimeout(ctx, 3*time.Second)
defer cancel()
```

При этом, если **родительский контекст отменён**, все **дочерние** тоже.

## Когда использовать `context.Context`?
### ✅ **Подходит для:**
- Управления временем выполнения горутин.
- Тайм-аутов HTTP-запросов, базы данных, RPC-вызовов.
- Передачи метаданных (ID запроса, токены).
### ❌ **Не подходит для:**
- Глобального хранения данных (лучше передавать в аргументах).
- Долгоживущих объектов (контексты должны создаваться и умирать вместе с операциями).
## Итог
- **`context.WithCancel()`** — отменяет контекст вручную.
- **`context.WithTimeout()`** — отменяет контекст через `N` секунд.
- **`context.WithDeadline()`** — завершает контекст в **указанный момент**.
- **`context.WithValue()`** — передаёт значения (но не стоит злоупотреблять).

Контекст полезен, если работаешь с **горутинами, HTTP-серверами, базами данных и API**.