### C++ Classes and Objects

- A class is a blueprint for the object.
- When a class is defined, only the specification for the object is defined; no memory or storage is allocated.
- We can access the data members and member functions of a class by using a . (dot) operator.

``` c++
// initiatin a class
class ClassName {
   // some data
   // some functions
};

// syntax to define an object
ClassName object_name;
```

##### C++ Constructors

- A constructor is a special member function that is called automatically when an object is created.
``` c++
class  Wall {
  public:
    // create a constructor
    Wall() {
      // code
    }
};
```
**C++ default constructor**

- A constructor with no parameters is known as a default constructor.
``` c++
// declare a class
class  Wall {
  private:
    double length;

  public:
    // default constructor to initialize variable
    Wall()
      : length{5.5} {
      cout << "Creating a wall." << endl;
      cout << "Length = " << length << endl;
    }
};
```

**Defaulted Constructor**

- When we have to rely on default constructor to initialize the member variables of a class, we should explicitly mark the constructor as default.
- If we want to set a default value, then we should use value initialization.
``` c++
// declare a class
class  Wall {
  private:
    double length {5.5};

  public:
    // defaulted constructor to initialize variable
    Wall() = default;
    
    void print_length() {
    	cout << "length = " << length << endl;
    }
};
```

**C++ Parameterized Constructor**

- A constructor with parameters is known as a parameterized constructor.

``` c++
// declare a class
class Wall {
  private:
    double length;
    double height;

  public:
    // parameterized constructor to initialize variables
    Wall(double len, double hgt)
      : length{len}
      , height{hgt} {
    }

    double calculateArea() {
      return length * height;
    }
};
```
- The member initializer list is used to initialize the member variables of a class.
- The order or initialization of the member variables is according to the order of their declaration in the class rather than their declaration in the member initializer list.

**C++ Copy Constructor**

- The copy constructor in C++ is used to copy data from one object to another.

``` c++
// copy constructor with a Wall object as parameter
// copies data of the obj parameter
Wall(const Wall& obj)
    : length{obj.length}
    , height{obj.height} {
}

// copy contents of wall1 to wall2
Wall wall2 = wall1;
```

**C++ Default Copy Constructor**

- If we have not defined a copy constructor. The compiler use the default copy constructor to copy the contents of one object of the Wall class to another ( the above will be done without a constr but the order will be default) .

