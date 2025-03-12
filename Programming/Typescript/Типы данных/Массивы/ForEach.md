# Синтаксис
```ts
Array<string>.forEach(callbackfn: (value: string, index: number, array: string[]) => void, thisArg?: any): void

@param callbackfn функция которую цикл вызывает для каждого элемента массива
```
# Возвращает
```ts
void
```

> [!Пример]
> ```ts
> [1, 5].forEach((el) => {
>	console.log(el) // 1 - при певой итерации, 5 - при второй
>})
> ```