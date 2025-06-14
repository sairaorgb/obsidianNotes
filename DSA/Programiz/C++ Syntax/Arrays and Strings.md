``` c++
dataType arrayName[arraySize];
```

**C++ Intialization**

``` c++
int x[6] = {19, 10, 8, 17, 9, 15};
int x[] = {19, 10, 8, 17, 9, 15};

// store only 3 elements in the array
int x[6] = {19, 10, 8}; // remaining are zeroes
```

- Array indexing is done and it checks for out of bound error. 
-  multi dimensional arrays are declared with sizes of all dims mentioned.
-  a drawback with c-style arrays is one array cant be assigned to another directly.
``` c++
int test[2][3] = {2, 4, 5, 9, 0, 19};
int  test[2][3] = { {2, 4, 5}, {9, 0, 19}};
```

**Passing array to a Function**

``` c++
returnType functionName(dataType arrayName[]) {
    // code
}

// function call
functionName(basepointer);

// multi dimensional array (col size is imp)
returnType functionName(dataType arrayName[][3]) {
    // code
}
```

- C-style arrays usually dont contain size inside them. moreover when an array is passed down a function it is passed as a pointer which the function uses for navigating through the array .

 ### **C++ Strings**
 - Stirngs can be written in two ways
	 - C-style strings
	 - String object of string class

**C - style strings**

- C-strings are arrays of type `char` terminated with a null character, that is, `\0`
	``` c++
	char str[4] = "C++"; \\ size of 4 
	char str[] = {'C','+','+','\0'};
	char str[4] = {'C','+','+','\0'};
	```
- Cin to ignore buffer caused by spaces during input.
``` c++
 cin.get(str, 100);
```

**String object**
- String objects have no fixed length and can be extended/
-  getline(cin, str) is used to account for space while taking input.

``` c++
\\ overloaded functions
void display(char *);
void display(string);

\\function calls
display(str);
display(str);
```
- many class functions pertaining to string class are available to use.
![[Pasted image 20250528105927.png]]

- if find fails then the variable initiated is returned string :: npos value.

``` c++
string str = "Hello world, wonderful world!";

// find the first occurrence
    size_t first_occurrence = str.find("world");

// find the last occurrence
    size_t last_occurrence = str.rfind("world");

//append the string
    str.append(" Have a good day!");

// insert "beautiful" at the 13th index
    str.insert(13, " beautiful");

// erase five characters starting from the seventh index
    str.erase(7, 5);

// replace two characters with "Earth"
// starting from the seventh index
    str.replace(6, 2, "Earth");

// lexographic comparison
str.compare(str1) // returns 1 if str is greater
```
