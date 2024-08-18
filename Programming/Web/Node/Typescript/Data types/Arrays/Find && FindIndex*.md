# Find
# Синтаксис
```ts
Array<any>.find<any>(predicate: (value: number, index: number, obj: number[]) => value is any, thisArg?: any): any

@param predicate коллбек функция вызываемая для каждого элемента
```
# Возвращает
```ts
Первый элемент для которого коллбек функция вернет true
```

> [!Пример]
> ```ts
> foobar.find((obj) => obj.value === 123);
> ```

# FindIndex