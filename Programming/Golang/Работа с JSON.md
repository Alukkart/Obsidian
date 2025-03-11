В Go пакет `encoding/json` позволяет кодировать (`marshal`) и декодировать (`unmarshal`) JSON-данные. Разберём основные аспекты.

## ## Кодирование (структура → JSON)
Используется `json.Marshal()`.

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name  string `json:"name"`
	Age   int    `json:"age"`
	Email string `json:"email,omitempty"` // Не будет в JSON, если пустой
}

func main() {
	user := User{Name: "Иван", Age: 25}
	jsonData, _ := json.Marshal(user) // Кодируем
	fmt.Println(string(jsonData))     // {"name":"Иван","age":25}
}
```

**Флаги тегов:**
- `json:"имя"` → задаёт имя поля в JSON.
- `json:"-"` → исключает поле.
- `json:",omitempty"` → исключает, если `nil` или пустое.

## Декодирование (JSON → структура)
Используется `json.Unmarshal()`

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name  string `json:"name"`
	Age   int    `json:"age"`
	Email string `json:"email"`
}

func main() {
	jsonStr := `{"name":"Иван","age":25,"email":"ivan@example.com"}`
	var user User
	json.Unmarshal([]byte(jsonStr), &user) // Декодируем JSON
	fmt.Println(user)                      // {Иван 25 ivan@example.com}
}
```

## ## JSON в `map[string]interface{}`
Когда структура неизвестна, можно распарсить в `map[string]interface{}`:

```go
jsonStr := `{"name":"Иван","age":25}`
var data map[string]interface{}
json.Unmarshal([]byte(jsonStr), &data)

fmt.Println(data["name"].(string)) // Иван
fmt.Println(int(data["age"].(float64))) // 25 (JSON числа всегда float64!)
```

**Особенность**: JSON числа превращаются в `float64`, их нужно приводить к `int`.

## JSON с `io.Reader` (например, из HTTP)
Когда JSON приходит из запроса (`io.Reader`), используем `json.NewDecoder()`:

```go
package main

import (
	"encoding/json"
	"fmt"
	"strings"
)

func main() {
	jsonStr := `{"name":"Иван","age":25}`
	reader := strings.NewReader(jsonStr)

	var user map[string]interface{}
	json.NewDecoder(reader).Decode(&user)

	fmt.Println(user) // map[name:Иван age:25]
}
```

## Потоковое кодирование JSON (`json.NewEncoder`)
Для записи JSON в `io.Writer` (например, HTTP-ответ):

```go
package main

import (
	"encoding/json"
	"os"
)

type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func main() {
	user := User{Name: "Иван", Age: 25}
	json.NewEncoder(os.Stdout).Encode(user) // {"name":"Иван","age":25}
}
```

## JSON с вложенными структурами

```go
type Address struct {
	City  string `json:"city"`
	State string `json:"state"`
}

type User struct {
	Name    string  `json:"name"`
	Age     int     `json:"age"`
	Address Address `json:"address"`
}

jsonStr := `{"name":"Иван","age":25,"address":{"city":"Москва","state":"RU"}}`
var user User
json.Unmarshal([]byte(jsonStr), &user)
fmt.Println(user.Address.City) // Москва
```

## JSON с массивами

```go
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

jsonStr := `[{"name":"Иван","age":25},{"name":"Анна","age":30}]`
var users []User
json.Unmarshal([]byte(jsonStr), &users)

fmt.Println(users[1].Name) // Анна
```

## JSON с необязательными полями (`*string`, `*int`)
Если поле может быть `null`, лучше использовать `*тип`.

```go
type User struct {
	Name  string  `json:"name"`
	Email *string `json:"email"`
}

jsonStr := `{"name":"Иван","email":null}`
var user User
json.Unmarshal([]byte(jsonStr), &user)

fmt.Println(user.Email == nil) // true
```

## Итог
- `json.Marshal()` → структура → JSON
- `json.Unmarshal()` → JSON → структура
- `json.NewEncoder().Encode()` → потоковое кодирование
- `json.NewDecoder().Decode()` → потоковое декодирование
- `json:",omitempty"` → пропуск пустых значений
- `map[string]interface{}` → для динамического JSON
- `*тип` → если возможен `null`