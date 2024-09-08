
```c++
#include <iostream> // input output builtin lib
using namespace std; // lib/module import

int main() // default function
{
	int a {0}; // variable definition
	setlocale(LC_ALL, ""); // sets language standard
	cin >> a; // console input
	cout << a; // console output
}
```

**Заметки**

Используется статическая типизация
Нет автоматического сборщика мусора (осторожнее с памятью)
Многие фишки различаются в разных компиляторах
Импортировать библиотеки на глобальном уровне - плохая идея
using namespace std = bad practice
Именно в проектах, где используется много модулей и библиотек

**Оглавление**
- [[Variables]]
- [[Constructions]]
- [[Types]]