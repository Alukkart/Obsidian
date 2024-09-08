
**Строковые операции**

```cpp

// иначе будет выводить символы даже после конца строки
char chars1[] { 'h', 'e', 'l', 'l', 'o', '\0' };
char chars2[] { "hello" }; // одинаковая запись

std::string test { "helloW" }; // тоже массив символов

cout << test; // helloW
cout << { 'h', 'e' }; // he╨J:╕╗☻
cout << { 'h', 'e', '\0' }; // he


// ограничение ввода по длине, знак '!' - сигнал завершения ( getline )
const int maxLength { 100 };
char chars3[] {};
std::cin.getline(chars3, maxLength, '!');

// инициализация через существующую переменную
std::string hello { "hello world" };
std::string hi { hello }; // hello world

std::string test2 { 4, 'd' }; // d*4 = dddd

// срезы при инициализации
std::string hi2 { hello, 5 }; // hello ( 5 - конец )
std::string hi3 { hello, 6, 5 }; // world ( 6 - начало, 6+5 - конец )

// изменение символов строки
std::string test3 { "wassup" };
test3[5] = 's'; // т.к по-сути это тоже массив символов
cout << test3; // wassus

// итерация по строке
for (char &c : test3) {
	if (c == 's') {
		c = 'c';
	}
}

cout << test3; // waccuc

// размер
test3.length(); // 6
test3.size(); // 6
test3.empty(); // false ( пустая ли строка )

// объединение строк
cout << "hello ""world"; // если без переменных
cout << test3 + " " + hi2; // waccuc hello ( с переменными )\

// преобразование литерала в объект string
// ! WARNING ! не работает в некоторых компиляторах

#include <iostream>
#include <string>
using namespace std::string_literals;

int main() {
	std::string message{ "hello "s + "world"s + "!"s};
	cout << message
}

// raw string

cout << "Name: \t\"Test\"\nAge: \t\"42\"" // неудобно и непонятно
std::string test4 {R"(Name: "Test" 
Age:  "42")"}; // используется R как обозначение raw, "()" вместо "" 

>> Name: "Test"
>> Age:  "42"
```