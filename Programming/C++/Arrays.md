**Массивы**

```cpp
int numbers[] { 1, 2, 3 };
int numbers2[3] { 1, 2, 3 }; // фиксированное кол-во элементов

cout << numbers // адрес памяти 1 элемента
cout << sizeof(numbers); // общее кол-во байт
cout << sizeof(numbers) / sizeof(numbers[0]); // кол-во элементов

int msize = sizeof(numbers) / sizeof(numbers[0]);

for (int i = 0; i < msize; i++) {
	cout << numbers[i]; // напишет каждый элемент
}

for (int n : numbers) {
	cout << n; // напишет каждый элемент
}

int nums2[][2] { {1, 2}, {3, 4} } // многомерный массив
// размер всего массива
int msize2 = sizeof(numbers) / sizeof(numbers[0][0]); 

for (auto &ref : nums2) {
	for (int n : ref) {
		cout << n << " ";
	}
	cout << endl;
}

// Вывод
>> 1 2
>> 3 4

// Ориентация по массиву также доступна при помощи указателей

int *numsp = nums;
cout << *numsp << endl; // 1
numsp++
cout << *numsp << endl; // 2
```