```cpp
#include <iostream>
#include <map>

using namespace std;
typedef double (*calculateType) (double a, double b);
double (*calculate) (double a, double b);

double add(double a, double b) {
    return a + b;
}

double substract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    return a / b;
}

int performOperation(std::map<char, calculateType> *operations) {
    double a = 0;
    double b = 0;
    
    int lastInput = 1;
    char operation;

    cout << "Type in operation: ";
    cin >> operation;
    
    if (!((*operations)[operation])) {
        cout << "Wrong operation!" << endl;
        return 0;
    }
    
    cout << "Type in first number: ";
    cin >> a;
    
    cout << "Type in second number: ";
    cin >> b;
    
    calculate = (*operations)[operation];
    cout << "Result is: " << calculate(a, b) << endl;
    
    cout << "Continue: ";
    cin >> lastInput;
    
    if (lastInput) { performOperation(operations); } 
    else { return 0; }
    
    return 0;
}

int main() {
    std::map<char, calculateType> operations {
        {'+', add}, {'-', substract}, {'*', multiply}, {'/', divide}
    };
    
    std::map<char, calculateType> *operationsptr = &operations;
    
    performOperation(operationsptr);
}
```

Right now without any graphical shell, still learning