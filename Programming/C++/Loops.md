
**Циклы**

```cpp
int i {0};

// цикл while
while (i < 10) // инициализатор
{ // тело цикла
	i++;
	cout << i << endl;
}

// цикл for
for (int j {0}; j < 10; i++) {
	cout << j << endl;
}

i = 0;

for (; i < 10;) { // пропуск условий
	i++;
	cout << i << endl;
}

int sum {0}
for (int j {0}; j < 10; sum += j++); // краткая запись

cout << sum //45

int nums[] { 1, 2, 3 }

for (int n : nums) {
	cout << n << endl; // вывод каждого элемента массива
}

```