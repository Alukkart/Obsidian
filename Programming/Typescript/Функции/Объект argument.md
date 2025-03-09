Объект **`arguments`** — это подобный массиву объект, который содержит аргументы, переданные в функцию.


> [!NOTE] **Примечание**
> Если вы пишете ES6-совместимый код, то лучше использовать [[Остаточные парамтры]]


> [!NOTE] Примечание
> "Подобный массиву" означает, что `arguments` имеет свойство `lenth`, а элементы индексируются начиная с нуля. Но при этом он не может обращаться к встроенным методам `Array`, таким как `forEach` или `map`.

```ts
function func1(a, b, c) {
  console.log(arguments[0]);
  // Expected output: 1

  console.log(arguments[1]);
  // Expected output: 2

  console.log(arguments[2]);
  // Expected output: 3
}

func1(1, 2, 3);
```
