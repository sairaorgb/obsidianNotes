#### Objects 

- Objects , a non primitive data type , is  used to store keyed collections of various data and more complex entities.

``` js
// Intiatilization
let user = new Object(); // "object constructor" syntax
let user = {}; // object literal syntax

// Declaration 
let user = {     
  name: "John",  
  age: 30,
  "likes birds": true // multiword property name must be quoted  
};

// Accessing properties (returns undefined in case of failure)
user.name         // dot operator
user.likes birds  // [doesnt work]
user["likes birds"]

// deleting properties
delete user.name 

// declaration using square br
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};

//property value shorthand
let user = {
  name,  // same as name:name
  age: 30
};

// property exsistence 
"propertyName" in object     // returns boolean

```

- keys can be keywords , numbers and many more . 
- for .. in loop can be used to navigate through keys.
- integer properties are sorted, others appear in creation order. 
- primitives create new space to copy variables of their type. But objects just add a reference to the object to new variable. objects declared by assigning another object are strictly equal .
``` js 
// duplicating object data into another
Object.assign(dest, ...sources)

// deep cloning or stuctured cloning
// if source object is nestesd then the target contains ref of nested objects instead of data
// accounts for circular references
// fails at functional variables

let user = {
  name: "John",
  sizes: { height: 182, width: 50 }
};

let clone = structuredClone(user);
```

- **Garbage collection** is a mechanism where in prog languages like js , space is automatically cleared . It works on the concept of reachability. 

- A function that is a property of an object is called its method. To access the object, a method can use the `this` keyword.
- `this` is not bound . A function can use this and when it gets assigned to an object, it gets its context otherwise it stays undefined. 
