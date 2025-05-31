- A structure is a collection of variables of different data types and member functions under a single name ( user defined data type).
- The members of a structure variable are accessed using a dot (.) operator.
- In C++, structures can also have member functions.
- Dynamic allocation is not needed since during the variable declaration , the space is allocated. 

``` c++
struct Person
{
    string first_name;
    string last_name;
    int age;
    float salary;
};

// variable with struct is created
Person bill;
bill.age = 59;
Person p {"John", "Doe", 22, 145000};
```

#### Passing and returning structs from function

``` c++
// declare function with
// structure variable type as an argument
void display_data(const Person&);

// define function to return structure variable
Person get_data() {
	Person p;
	
	string first_name;
	string last_name;
	int age;
	float salary;
	
	// statements for getting these var from cin
	// return structure variable
	return Person{first_name, last_name, age, salary};
}
```

#### C++ pointers to struct

- Pointers can be defined to user defined data types similar to the built in data types.
- We can use the arrow (->) operator to access member variables and member functions of a structure variable through a pointer.

``` c++
Distance d;
Distance* ptr = &d;

// dot operator works on dereferenced pointer
(*ptr).feet;

// arrow operator
ptr->feet;
```

### C++ Enums

- An enumeration is a user-defined data type that consists of integral constants.

``` c++
enum season { spring, summer, autumn, winter };

season thisSeason;
thisSeason = summmer;

enum season 
{   spring = 0, 
    summer = 4, 
    autumn = 8,
    winter = 12
};

season thisSeason;
thisSeason = summmer;
// thisSeason now contains int value 4


```