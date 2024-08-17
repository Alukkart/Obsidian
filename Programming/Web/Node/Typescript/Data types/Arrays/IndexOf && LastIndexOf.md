# indexOf
Возвращает индекс первого найденного элемента
```ts
indexOf(searchElement: any, fromIndex?: number): number
console.log([1, 5].indexOf(5)) // 1
console.log([1, 5, 5].indexOf(5, 2)) // 2 т.к указан fromIndex
```
# LastIndexOf
Возвращает индекс последнего найденного элемента
```ts
indexOf(searchElement: any, fromIndex?: number): number
console.log([1, 5, 5].lastIndexOf(5)) // 2
console.log([1, 5, 5].lastIndexOf(5, 2)) // 2 т.к указан fromIndex
```