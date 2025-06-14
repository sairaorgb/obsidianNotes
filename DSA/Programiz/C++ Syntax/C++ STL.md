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

#### Extrra Stuff

- Containers usually occupy more space than their size * data type size , this is to include the metadata which is an overhead containing stuff. 
-  For Sequential containers , an iterator can be added with an integer to get a valid element of that container . wherease in other containers , this does not work cause elements dont occupt consecutive locations and the iterator needs to be incremented with ++ to get next location. 
- Unlike vectors or other containers, we cannot use a ranged for loop to iterate through a stack. This is because the STL stack is an STL Container Adapter, which provides restrictive access to make it behave like a standard stack data structure.


###### C++ STL Array

`std::array` is a container class that encapsulates fixed size arrays.

- Array declaration
``` c++
#include <array>
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

###### C++ STL Vectors

- In C++ ,vectors are used to store elements of similar data types. However, unlike arrays, the size of a vector can grow dynamically.
-  We can also use the insert() and emplace() functions to add elements to a vector.

``` c++
std::vector<T> vector_name;

//Intialization

vector<int> vector1 = {1, 2, 3, 4, 5};   // Initializer list
vector<int> vector2 {1, 2, 3, 4, 5};     // Uniform initialization
vector<int> vector3(5, 12);          // first arg is size and second arg is value
vector<int> vector(3);               // 3 size with element 0

// Adding elements to vector
num.push_back(6);

// Accessing elements of a vector
num.at(index);   // checks for bounds
num[index];       // doesnt check for bounds

// deleting elements
num.pop_back();

// iterators
vector<int>::iterator itr;
itr = num.begin();

```
- other functions include size , clear , front , back ,empty and capacity 
- adding element usually takes O(1) time, in worst case it is O(n) and rewrites entire thing to new available space.

###### C++ STL Lists

- In C++, the STL list implements the doubly-linked list data structure. 
``` c++
std::list<Type> list_name = {value1, value2, ...};

// initialization
list<int> numbers = {1, 2, 3, 4, 5};

// adding elements
numbers.push_front();
numbers.push_back();

// accessing elements
numbers.front();
numbers.back();

// deleting elements
numbers.pop_front();
numbers.pop_back();

```
- other functions include reverse , sort , unique (removes consecutive duplicate elements), empty , size , clear and merge.
- Elements do not have consecutive addresses from their prev elements . so during traversal , incrementing of iterator only makes sense and adding integer to iterator does not work as it does like vectors.

 **C++ Forward List**
 - It is implemented with linked list and it only allows elements to be added or removed at the front.
 
 **C++ Queue**
 
- The queue data structure follows the FIFO (First In First Out) principle where elements that are added first will be removed first.

Methods	   Description

push()	       Inserts an element at the back of the queue.
pop()	       Removes an element from the front of the queue.
front()	       Returns the first element of the queue.
back()	       Returns the last element of the queue.
size()	       Returns the number of elements in the queue.
empty()	   Returns true if the queue is empty.



##### C++ Map

- In C++, maps are associated containers that hold pairs of data.
``` c++
// create a map with integer keys and string values
std::map<int, string> student = {{1,"Jacqueline"}, {2,"Blake"}, {3,"Denise"}};

// adding elements
map_name[key] = value;  // using the [] operator
// using the insert() and make_pair() functions
map_name.insert(std::make_pair(key, value));

// declare map iterator
map<int, string>::iterator iter;

```

- Operation	Description
  insert()         adds an element (key-value pair) to the map
  erase()	      removes an element or range of elements from the map
  clear()	      removes all the elements from the map
  find()	      searches the map for the given key
  size()	      returns the number of elements in the map
  empty()	      returns true if the map is empty.

 
##### C++ Set

- Sets are STL containers that store unique elements of the same type in a sorted manner. sets have unique elements , immutable , sorted and associative properties.
- element traversal cannot be done by using indexes. traversal is done directly on keys.
- Also, the unordered set is implemented as a hash table data structure whereas the regular set is implemented as a binary search tree data structure.
- **multiset** allows set to have duplicate values. Erase function in this case deletes all instances of the key.

- Operation	Description

  insert()	       Insert elements into a set.
  erase()	       Delete elements from a set.
  clear()	       Remove all the elements from a set.
  empty()	       Check if the set is empty. 
  size()	       Returns the size of the set.
  count()           returns number of instances of key

``` c++
set<data_type> set_name = {key1, key2, key3, ...};

// stores in desc order
set<int, greater<int>> my_set = {5, 3, 8, 1, 3};

// add values to the set
my_set.insert(10);

// delete values from the set
my_set.erase(10);
```