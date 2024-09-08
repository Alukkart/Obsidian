# **Операции сравнения**
```cpp
int a { 10 };
int b { 15 };

bool c { a == b }; // false
bool d { a == 10 }; // true

bool c { a < b };
bool c { a > b };
bool c { a <= b };
bool c { a >= b };
bool c { a != b };

std::cout << c; // c = 1
std::cout << std::boolalpha << c; // c = true

int d { true }
int e { false }

std::cout << std::boolalpha << (d && e); // false (AND)
std::cout << std::boolalpha << (d || e); // true (OR)
std::cout << std::boolalpha << (d ^ e); // true (XOR)
```

**Конструкции**

```cpp
#include <iostream>
using namespace std;

int main()
{
	int a {8};
	int b {0};
	
	if (a == 8) {
		cout << a << endl;
		a = 7;
	}
	
	if (a > 8) {
		cout << a << " pass" << endl;
	}
	else if (a < 5) {
		cout << a << " pass2" << endl;
	}
	else {
		cout << a << " pass3" << endl;
	}
	
	// сокращённая запись
	if (a) cout << a << " a = true" << endl; // a != 0
	if (b) cout << b << " b = false" << endl; // b == 0
	
	if (int c { a - b }; c > b) { // инициализация
		cout << "pass4" << endl;
	}
}
```

**Операторы**

```cpp
int a { 10 }
int b { 12 }

// ! --
int c = a > b ? a + b : a - b; 

// одинаковые записи

if (a > b) {
	cout << a + b
}
else {
	cout << a - b
}
// ! --

// сокращённая запись
switch(a)
{
	case 10:
		cout << "pass1";
		break; // иначе будет выполнять дальше выполнять действия
	case 12:
		cout << "pass2";
		break;
	default: // если ничего не прошло
		cout << "pass3";
		break;
}

// полная запись
switch(int k {2}; a) // инициализация
{
	case 10:
	{
		cout << "pass" << k;
		break;
	}
}
```

**Связанное**
- [[Loops]]