# Синтаксис
```ts
String.includes(searchString: string, position?: number): boolean

@param searchString — Искомая строка
@param position — Индекс до которого ведется поиск.

Array<any>.includes(searchElement: any, fromIndex?: number): boolean

@param searchElement — Искомый элмемент.
@param fromIndex — Индекс массива с которого начинается поиск.
```
# Возвращает
```
boolean
```

> [!Пример для сток]
> ```ts
> console.log('1221asd11'.includes('asd')) // true
> console.log('122a1sd11'.includes('asd')) // false
> ```

> [!Пример для массивов]
> ```ts
> console.log([1, 234, 52].includes(52)) // true
> console.log([1, 234, 52].includes(500)) // false
> ```