C++ STL has 3 major components:

- Containers
- Iterators
- Algorithms

#### C++ STL Containers

A container is an object that stores a collection of objects of a specific type.

1. **Sequence containers:**
	They allow us to store elements that can be accessed in sequential order.
	Internally, sequential containers are implemented as arrays or linked lists data structures.
	- Array
	- Vector
	- Queue
	- Deque
	- Forward_list
	- List

2. **Associative containers:**
	Associative containers allow us to store elements in sorted order.
	Internally, they are implemented as binary tree data structures.
	- Set
	- Multiset
	- Map
	- Multimap

3. **Unordered associative containers:**
	They provide the unsorted versions of the associative container.
	Internally, unordered associative containers are implemented as hash table data structures.
	- Unordered_set
	- Unordered_multiset
	- Unordered_map
	- Unordered_multimap
	
**Container Adapters**

- Container Adapters take an existing STL container and provide a restricted interface to make them behave differently. (example Stack)
- Container adaptor restricts assigning values directly to the container.
- Navigating through also requires manual work cause iterators dont work.

#### C++ STL Iterators
Iterators are objects that are used to access elements of a container.
``` c++
vector<int>::iterator it;
```

#### C++ STL Algorithms

In C++, we can use the Standard Template Library to implement some of the commonly used algorithms.
- Sorting algorithms
- Searching algorithms
- Copying algorithms
- Counting algorithms


###### C++ STL Array

`std::array` is a container class that encapsulates fixed size arrays.

- Array declaration
``` c++
#include <array
// T is the type and N is num of entites
std::array<T, N> array_name;
```

-  Intialization
``` c++
// initializer list
std::array<int, 5> numbers = {1, 2, 3, 4, 5};
// uniform initialization
std::array<int, 5> marks {10, 20, 30, 40, 50};
```
- Accessing Elements
``` c++
// using [] operator - does not check for out of bound
std::cout << num[0]
// using at operator - check for bounds
std::cout << n.at(0)
```
- Additonal Properties 
``` c++
num.size();
num.empty();

array<int, 5> a;
a.fill(1); // fills the array with 1

```
- STL stuff like iterators , algorithms works well with STL arrays.
- C-style arrays does not allow assignment whereas this does.
``` c++
int a[5] = {1, 2, 3, 4, 5};
// Error: invalid array assignment
int b[5] = a[5];

std::array<int, 5> a = {1, 2, 3, 4, 5};
// Correct: std::array supports assignment
std::array<int, 5> b = a;
```
