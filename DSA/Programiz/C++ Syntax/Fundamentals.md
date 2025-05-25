#### Keywords
Keywords are **predefined** words that have special meanings to the compiler. All keywords must be written in lowercase.

#### Identifiers
Identifiers are the unique names given to variables, classes, functions, or other entities by the programmer.
- Identifiers can be composed of letters, digits, and the underscore character.
-  It must begin with either a letter or an underscore.

#### Variables
variable is a container (storage area) to hold data.

#### Constants
we can create variables whose value cannot be changed. For that, we use the `const` keyword. A constant can also be created using the `#define` preprocessor directive.

#### Literals
Literals are data used for representing fixed values.

1. **Integers**
	An integer is a numeric literal(associated with numbers) without any exponential part.
	3 types of integer literals 
	- decimal (base 10)
	- octal      (base 8)            -     021 , 078 , 033
	- hexadecimal (base 16)  -      0x67, 0x3a , 0x53f
	
2. **Floating Point Literals**
	It is a numeric literal with exponent part.
	
3. **Characters**
	A character literal is created by enclosing a single character inside single quotation marks.
	
4. **String**
	A string literal is a sequence of characters enclosed in double-quote marks.

#### Data Types
Data types are declarations for variables. This determines the type and size of data associated with variables.

![[Pasted image 20250523123241.png]]
 
 - C++ int  - **-2147483648 to 2147483647**
 - C++ float and double
 - C++ Char 
 - C++ wchar_t - 2 bytes - L' '
 - C++ bool
 - C++ Void

#### Type Modifiers
We can further modify some of the fundemental data types by usign type modifiers.

- signed
- unsigned
- short 
- long
![[Pasted image 20250523133203.png]]
For elaborate details on the way which type modifier can be used with these data types you can browse through https://www.programiz.com/cpp-programming/type-modifiers

#### Constants
The `const` keyword is used to specify that the value of a variable cannot be changed anywhere in the program. 
const variables should be declared during compile time but not in run time.

###### Const references

- Constant reference to a Constant variable
``` C++
const double PI = 3.14;
const double &PI_REF = PI;
```

- Constant reference to a non-Constant variable
``` c++
int sum(const vector<int> &nums) {
    // function body
}
```
Through a constant reference , just the reference is passed as argument which'll save memory and avoid modification of nums.

###### Const and pointers

- **Pointers to a Const**
	A pointer to a const is a pointer variable that points to a constant variable. These pointers
	- allow us to change the address the pointer is pointing to,
	- don't allow us to change the value stored in those constant variables.
	``` c++
	
    const int TOTAL_MONTHS = 12;
    const int *MONTHS_PTR = &TOTAL_MONTHS;
  
    cout << "TOTAL_MONTHS: " << TOTAL_MONTHS << endl;
    cout << "*MONTHS_PTR: " << *MONTHS_PTR << endl;

    const int HALF_MONTHS = 6;
    MONTHS_PTR = &HALF_MONTHS;
    cout << endl;
  
    cout << "HALF_MONTHS: " << HALF_MONTHS << endl;
    cout << "*MONTHS_PTR: " << *MONTHS_PTR;
```

	We'll get an error if we try to modify the value at MONTHS_PTR.
	
- **Const Pointer**
	A **const pointer** is a pointer in which we can change the value pointed by the pointer, but cannot change the variable it points to.
	``` C++
	string country1 = "Nepal";
    string country2 = "USA";
  
    cout << "Initially, country1: " << country1 << endl;
    string *const PTR1 = &country1;

    *PTR1 = country2;  
    cout << "Finally, country1: " << country1;
  
    return 0;
```

	But we'll get an error if we try changing the variable pointed by PTR1 from country1 to country2

-  **Const Pointer to a Const**
	A const pointer to a const is a pointer through which we can neither change the variable it points to nor its value.
	
	``` c++
    const string COUNTRY1 = "Nepal";
    const string *const PTR = &COUNTRY1;
    cout << *PTR;
```

###### Const Expression

They are expressions whose:
- value cannot change,
- and can be evaluated at compile time.
``` c++
#include <iostream>
using namespace std;

// a constexpr function that
// returns nth fibonacci number
constexpr int fib(int n) {

    // returns n if n <= 1
    // else, recursively calls fib(n - 1) + fib(n - 2)
    return n <= 1 ? n: fib(n - 1) + fib(n - 2);   
}

int main() {

    // two constexpr variables that store
    // the return values of constexpr function
    constexpr int fibonacci_five = fib(5);
    constexpr int fibonacci_ten = fib(10);

    cout << "fib(5) : "<< fibonacci_five << endl;
    cout << "fib(10) : "<< fibonacci_ten;

    return 0;
} ```



