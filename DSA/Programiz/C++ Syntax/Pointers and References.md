#### C++ pointers

 - if var is a variable them &var is the address of it and represented in hexadecimal form.
 -  * (& var) is the dereferencing operator. 
 
 ```
int *ptr;
int arr[5];
ptr = arr;

ptr + 1 is equivalent to &arr[1];
ptr + 2 is equivalent to &arr[2];

*(ptr + 1) is equivalent to arr[1];
*(ptr + 2) is equivalent to arr[2];
```
#### C++ references

In C++, we use a reference to create an alias for a variable

- We must initialize references at the time of declaration.
- Once we create a reference to a variable, it cannot be changed to refer to another variable. 
``` c++
string& ref_city = city;

// modify the variable using reference
ref_city = "New York";

// incorrect code [reference not initialized]
string& ref_city;
ref_city = city;

```
- arguments of a function can be passed down as actual value , reference and also pointer
``` c++
// function that takes value as parameter
void func1(int num_val) {
    // code
}

// function that takes reference as parameter
void func2(int& num_ref) {
    // code
}

// function to add two numbers using const references
int add(const int& num1, const int& num2) {
    return num1 + num2;
}

// function definition to swap numbers
void swap(int* n1, int* n2) {
    int temp;
    temp = *n1;
    *n1 = *n2;
    *n2 = temp;
}
```
- operations involving referenced variables are retained to original variables. whereas const refernces cannot be modified . 

#### Dynamic memory allocation

- In c++ , since the pointers are used and they just contain the address of variable and not responsible for dealing with stuff around that pointer . so random address does not work and memory needs to be allocated before initializing pointers and then deallocate to avoid memory leaks. 

 ``` c++
// declare an int pointer
int* point_var;

// dynamically allocate memory using the new keyword 
point_var = new int;

// assign value to allocated memory
*point_var = 45;

int* point_var = new int{45};

delete pointer_variable;
point_var = nullptr;

// memory allocation of num number of floats
ptr = new float[num];

// ptr memory is released
delete[] ptr;
ptr = nullptr;

```

