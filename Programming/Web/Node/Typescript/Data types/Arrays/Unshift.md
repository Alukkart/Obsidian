Добавляет элемент в начало массива
# Синтаксис
```ts
Array<any>.unshift(...items: any[]): number
```
# Возвращает
```ts
Новую длинну массива
```

> [!Пример]
> ```ts
> let a:number[] = [5]
> a.unsift(1)
> console.log(a) // [1, 5]
> ```