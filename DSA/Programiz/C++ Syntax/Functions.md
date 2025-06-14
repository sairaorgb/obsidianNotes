There are two types of function:

**Standard Library Functions:** Predefined in C++ 
**User-defined Function:** Created by users

``` c++
returnType functionName (parameter1, parameter2,...) {
    // function body   
} ```

- In C++, the code of function declaration should be before the function call. However, we can use the function prototype for a different approach .

``` c++
// function prototype
void add(int, int);

int main() {
    add(5, 3);
    return 0;
}

// function definition
void add(int a, int b) {
    cout << (a + b);
}
```

- During compile time , the entire code is broken into a syntax tree and then analyzed for various things .. one imp thing to note herer is that compiler needs a function definition or atleast a function prototype before a call is made for that.
#### Function default parameters

``` c++
// defining the default arguments
void display(char = '*', int = 3);

// function calls
display();
display('#');
display('$',5);
```
- Once we provide a default value for a parameter, all subsequent parameters must also have default values.
- If we are defining the default arguments in the function definition instead of the function prototype, then the function must be defined before the function call.

#### C++ function overloading

- In C++, two functions can have the same name if the number and/or type of arguments passed is different. 
``` c++
// same name different arguments
int test() { }
int test(int a) { }
float test(double a) { }
int test(int a, double b) { }
```
- return types might be same or different but the signature of args passed must be different.
errors happen due to possible conflicts caused during compilation.

#### C++ Inline function

- Each time an inline function is called, the compiler copies the code of the function to that call location. This optimizes the code a bit.

``` c++
\\ before compilation

inline void displayNum(int num) {
    cout << num << endl;
}

int main() {
    displayNum(5);
    displayNum(8);
    displayNum(666);

\\ after compilation

inline void displayNum(int num) {
    cout << num << endl;
}

int main() {
    cout << 5 << endl;
    cout << 8 << endl;
    cout << 666 << endl;
}
} ```

#### C++ recursion

A  function that calls itself is known as a recursive function. And, this technique is known as recursion.