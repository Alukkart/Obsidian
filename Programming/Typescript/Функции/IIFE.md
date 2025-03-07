Immediately-Invoked Function - функция, которая выполняется сразу после её создания это функция, которая выполняется сразу после её создания.

## Пример
```ts
// An Async IIFE
( async() => {
    
    const x = 1;
    const y = 9;

    console.log(`Hello, The Answer is ${x+y}`);

})();
```