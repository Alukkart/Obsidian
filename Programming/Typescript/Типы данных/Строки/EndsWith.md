Проверяет заканчивается ли строка переданной в аргумент строкой
# Синтаксис
```ts
String.endsWith(searchString: string, endPosition?: number): boolean

@param searchString - искомая строка
@param endPosition? - до какого индекса ведется поиск
```
# Возвращает
```ts
boolean
```

> [!Пример]
> ```ts
> console.log('asd5'.endsWith('5')) // true
> console.log('asd'.endsWith('5')) // false
> console.log('1asd5'.endsWith('5', 3)) // false
> ```