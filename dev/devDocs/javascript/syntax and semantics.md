
## Functions

- Unlike C, where functions are static blocks of code, in JS they behave more like **objects with executable behavior**.
- functions are **first-class objects**—they can be stored in variables, passed around, and have properties.
-  prototype defines properties and methods that should be shared across all instances created by that object.
#### JS Functions vs Constructor Functions

##### 🔹 Regular Functions

- Used for logic, returning values.
- Not used with `new`.
 ``` js
function add(a, b) {   
	return a + b; 
} 
```
##### 🔹 Constructor Functions

- Used with `new` to create objects.
- Can define shared methods via `.prototype`.
``` js 
function Car(model) {   
	this.model = model; 
	this.greet = function(){
		return `this car is of ${this.model}`;		
	}
} 
const c = new Car("Tesla"); 
```

- When you use new constructor function, JavaScript: 
	- Creates a new object.
	- Sets the prototype of that object(`__proto__`) to constructor.prototype.
	- Calls constructor with this pointing to the new object.
	- Returns the object.

-  `this` refers to the object being created.
- Use it to define instance properties and methods.
- methods written inside function are copied over to all the objects, but if we Use `.prototype.methodName` to define the methods the unwanted copy can be reduced. 

``` js
function Animal() {
  // instance-specific setup (not needed here, but useful if you had properties)
}

Animal.prototype.speak = function() {
  return 'Animal speaking';
};

function Dog() {
  Animal.call(this);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return 'Woof!';
};

const dog1 = new Dog();

console.log(dog1.speak()); // Animal speaking
console.log(dog1.bark());  // Woof!

```
- Using call() to Inherit Constructor
	- Syntax: `Parent.call(this, args)`
	- Reuses parent constructor logic in child.
	- Helps inherit **only** instance properties.

- Object.create(proto) creates a new object with its Prototype (i.e., _ _ proto  _ _ ) set to proto. 
	- Helps inherit methods related to prototype of parent widget.

- JavaScript Prototype Chain
	- Every object in JS has an internal link to another object called its **prototype**.
	- This chain continues until it reaches `null`.
	- If a property/method isn’t found on the object:
		- JS checks its prototype.
		- Then its prototype’s prototype. until it reaches null.
		- This chain is called the Prototype Chain.

- After the prototype of animal is assigned to dog , Dog will lose its constructor since its practically the prototype of animal . so the constructor part needs to be rewritten;
- prototype.constructor points to the function that created an object's prototype.
- 