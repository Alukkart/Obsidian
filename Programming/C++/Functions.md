# Синтаксис функций
```cpp
#include <iostream>

void testFunc() {
	std::cout << "Hi!";
}

int main() {
	testFunc();
}

// ! Или !

void testFunc();

int main() {
	testFunc();
}

void testFunc() {
	std::cout << "Hi!";
}

// Код компилируется сверху вниз, поэтому необходимо объявить функцию перед вызовом ( даже если вы не собираетесь её инициализировать )

// Есть авто типизация ( auto ), аргументы по умолчанию, возвращаемые значения.
// В функциях можно также изменять значения, на которые указывают ссылки и поинтеры

int testFunc2(int arg1, int &arg2, int arg3 = 1) {
	arg2 = arg1 + arg3;
	arg1++;
	return arg2 * 2;
}

int main()
{
    int t = 10;
    int t2 = 10;
    int res = testFunc2(t2, t);
    
    cout << t2 << endl; // 10
    cout << t << endl; // 11
    cout << res << endl; // 22
}

// Преобразование типов

void testFunc3(int arg1) {
	cout << arg1;
}

int main() {
	testFunc3( (double)3.14 ) // выводит 3, а не 3.14
}
```

# Указатели и функции

Подобная логика распространяется на все операции с указателями
Свойства указателей ( Например, как влияет const ) можно подробно рассмотреть здесь [[Variables]]

```cpp
#include <iostream>
using namespace std;

void testFunc2(int *arg1) {
	(*arg1)++;
	cout << *arg1 << " Local Env" << endl; // 11 Local Env
}

int main()
{
    int t = 10;
    int *tpoint = &t;
    testFunc2(tpoint);
    cout << t << " Global Env" << endl; // 11 Global Env
    // Тут работает как ссылка
}

```

```cpp
#include <iostream>
using namespace std;

void testFunc2(int *arg1) {
	int y = 2;
	arg1 = &y;
	(*arg1)++;
	cout << *arg1 << " Local Env" << endl; // 3 Local Env
}

int main()
{
    int t = 10;
    int *tpoint = &t;
    testFunc2(tpoint);
    cout << t << " Global Env" << endl; // 11 Global Env
	// В локальной среде можно изменить указатель, не повлияв на прошлое значение
}
```

# Рекурсия

По - сути можно использовать и рекурсии и циклы в своих проектах
Рекурсии могут перегрузить стек, но каждую итерацию намного легче контролировать
В циклах же проблема перегрузки не такая яркая, их преимущество - удобнее писать большие условия или последовательности действий, ставить задержки

```cpp
#include <iostream>
using namespace std;

int recFunc(int arg1) {
	if (arg1 > 1) {
		return arg1 * recFunc(arg1-1); // функция будет вызывать сама себя
	}
	else {
		return 1;
	}
}

int recFunc2(int *arg1) {
    if ((*arg1) <= 1) {
        return 1;
    }
    
    (*arg1)--;
    cout << (*arg1) << endl;
    
    return recFunc2(arg1);
}

int main()
{
    int test = 10;
    int *testp = &test;
    recFunc2(testp); // счёт от 9 до 1 (что-то вроде итератора)
	
	for (int i = 9; i > 1; i--) {
		cout << i << endl; // такой же счёт
	}
    
    cout << test << endl; // 1
    cout << recFunc(5) << endl; // 120
}
```

# Перегрузка функций

```cpp
#include <iostream>
using namespace std;

int sum(int a, int b) {
    return a + b;
}
double sum(double a, double b, double с) {
    return a + b + с;
}

int test1(const int *a) {
	return *a * *a;
}

int test1(int *a) {
	*a = *a * *a;
	return 0;
}

int main() {
    const int x = 10;
    int y = 11;
    
	const int *t = &x;
	int *g = &y;
	
	cout << test1(t) << endl; // 100
	cout << test1(g) << endl; // 0
	
	cout << *t << endl; // 10
	cout << *g << endl; // 121
	
	cout << sum(1, 2) << endl; // 3 ( использует int )
	cout << sum(1.2, 2.8, 2) << endl; // 6 ( использует double )
}

// Override datatype и const datatype работать НЕ будет!
// Не работает

int test(int a);
int test(const int a);
```

# Указатель на функцию
Удобно использовать для hashmap, где ключ - какой-то аргумент, значение - функция
Так можно сделать систему выбора различный действий, например, действия в калькуляторе

```cpp
#include <iostream>
using namespace std;

void hello();
void hello2();

int main() {
	void (*message) (); // перезапись функции
	
	message = hello;
	message(); // Hello
	message = hello2;
	message(); // Hello2
}

void hello() {
	cout << "Hello" << endl;
}

void hello2() {
	cout << "Hello2" << endl;
}

```