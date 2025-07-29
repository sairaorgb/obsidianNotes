### Intro 

#### Hello World! 

- js code run in  env like nodejs are compiled and run with node sample.js command, whereas it can be run in browser console too.
- We can use a < script> tag to add JavaScript code to a page.
- The type and language attributes are not required.
- A script in an external file can be inserted with < script src="path/to/script.js">< /script> or lightweight js code can be added inside script tag. (only one should be done)

#### Code Structure

- JavaScript interprets the line break as an “implicit” semicolon. This is called an automatic semicolon insertion.
- In most cases, a newline implies a semicolon. But “in most cases” does not mean always. 
``` js 
// js doesnt include semicolons here (+ creates a sense of incompleteness)
alert(3 +
1 + 2);

/* gives an error cause of square brackets 
not assuming semicolon before */

alert("Hello")
[1, 2].forEach(alert);
```

#### The modern mode, "use strict"

- Some stuff in older versions of js is deprecated, A special directive `use strict` is used at the top of script to let env know that this script used modern stuff.
- it is enabled by default in modern js. 

#### Variables

- A variable is a “named storage” for data.To create a variable in JavaScript, use the `let` keyword.
- var and let are almost same. 
``` js 
// commas enable to declare multiple vars on diff lines
let user = 'John',
age = 25,
message = 'Hello';
```
- The name must contain only letters, digits, or the symbols $ and _.
- To declare a constant (unchanging) variable, use const instead of let. A loose convention is to use capital case for compile time constants and small case for run time constants. 

#### Data types


There are 8 basic data types in JavaScript.

| Type          | Example                               | Notes                              |
| ------------- | ------------------------------------- | ---------------------------------- |
| **Number**    | `42`, `3.14`, `-0`, `NaN`, `Infinity` | Includes `NaN`, `Infinity`, etc.   |
| **String**    | `"hello"`, `'world'`                  | Sequence of characters             |
| **Boolean**   | `true`, `false`                       | Logical values                     |
| **Undefined** | `undefined`                           | Variable declared but not assigned |
| **Null**      | `null`                                | Explicit "no value"                |
| **BigInt**    | `123n`, `9007199254740991n`           | For very large integers            |
| **Symbol**    | `Symbol('desc')`                      | Unique identifiers                 |

- Special Values

| Value       | Type        | Description                                      |
| ----------- | ----------- | ------------------------------------------------ |
| `undefined` | `undefined` | Variable declared but not initialized            |
| `null`      | `object`    | Explicitly no value (JS quirk: type is `object`) |
| `NaN`       | `number`    | Invalid number result (e.g. `0 / 0`)             |
| `Infinity`  | `number`    | Result of positive overflow (e.g. `1 / 0`)       |
| `-Infinity` | `number`    | Result of negative overflow (e.g. `-1 / 0`)      |

- if there’s a NaN somewhere in a mathematical expression, it propagates to the whole result (there’s only one exception to that: NaN ** 0 is 1).
- `number` type cannot safely represent integer values larger than (2^53-1). Beyond which it starts to round them to precision. so BigInt is used. 
- No special char, they are string too. Double and single quote strings are same. But strings with Backticks allow embedding.
``` js 
let name = "John";
alert( `Hello, ${name}!` ); 
alert( `the result is ${1 + 2}` ); 
```

The `typeof` operator allows us to see which type is stored in a variable.

- Usually used as `typeof x`, but `typeof(x)` is also possible.
- Returns a string with the name of the type, like `"string"`.
- For `null` returns `"object"` – this is an error in the language, it’s not actually an object.

#### Interaction: alert, prompt, confirm

3 browser-specific functions to interact with visitors: (modal methods)

- `alert`  shows a message.
- `prompt` shows a message asking the user to input text. It returns the text or, if Cancel button or Esc is clicked, `null`.
- `confirm`  shows a message and waits for the user to press “OK” or “Cancel”. It returns `true` for OK and `false` for Cancel/Esc.

Values captured by these are strings. Be cautious about directly using them in other operations. 
we consider return value of alert to be undefined. 

#### Basic operators

- If either operand is a string, + → string concatenation. Else, both operands are coerced to numbers.
- any other operator converts strings to numbers and evaluate even if it's NaN.
``` js 
alert(2 + 2 + '1' ); // "41" and not "221"
alert('1' + 2 + 2); // "122" not '14'

// Unary + Converts non-numbers
alert( +true ); // 1
alert( +"" );   // 0

// chained assignments evaluate from right to left. 
a = b = c = 2 + 2;
```
 - The assignment operator in  x = value writes the value into x and then returns it.
 
 - **Postfix and Prefix** - prefix returns first and evaluates whereas suffix evaluates and then returns .. They have precedence same as arithmetic operators.
 ``` js 
Expression order matters:
let a = 1;
let b = a++ + ++a; // b = 1 + 3 = 4

Array indexing:
let arr = [10, 20]; let i = 0;
arr[i++] → 10, i = 1

Function arguments:
let x = 1;
func(x++, ++x); // Order is left to right, but values may surprise

Condition checks:
let i = 0;
if (i++ === 0) → true, i becomes 1 after

Avoid in complex expressions:
i++ + i++ + i → ❌ unclear, side effects stack
```

- Bitwise operators treat arguments as 32-bit integer numbers and work.

#### Type coercion

- To Number — Happens in math, comparisons
- space characters are trimmed off string. 
``` js 
Number(null)      → 0
Number(undefined) → NaN
Number(true)      → 1
Number(false)     → 0
Number("42")      → 42
Number("  ")      → 0
Number("")        → 0
Number("abc")     → NaN
Number(" \t \n ") → 0
Number([])        → 0
Number([1])       → 1
Number([1,2])     → NaN
Number({})        → NaN
```

- To String — Happens in `+` when one operand is a string.
``` js
String(null)       → "null"
String(undefined)  → "undefined"
String(true)       → "true" 
String(123)        → "123"
String([1,2,3])    → "1,2,3"
String({})         → "[object Object]"
```

##### Coercion table

| Operand Types       | Coercion Behavior                                  |
| ------------------- | -------------------------------------------------- |
| Number == String    | Convert String → Number                            |
| Boolean == Any      | Convert Boolean → Number                           |
| Object == Primitive | Convert Object → Primitive (`valueOf`, `toString`) |
| null == undefined   | `true`                                             |
| NaN == anything     | `false` (even `NaN == NaN`)                        |
| String == Object    | Object → Primitive, then String compared           |


#### Comparisons

- String comparison is made with lexographical analysis with unicode values. pitfall is smaller case have bigger unicode values than upper case. 
- In case of comparison operators If both operands are strings, → string comparison. Otherwise → both coerced to numbers (except undefined).

- `==` does type coercion before comparing unlike `===` . type coercion in js is too erratic and care is needed.
- **Truthy** values are the ones that are logical true and the opposite is **falsey**.

- To Boolean — Happens in conditions (e.g., if(x))
``` js
false, 0, -0, 0n, "", null, undefined, NaN 
// anything except them is truthy

"0" // truthy
```

- A strict equality operator === checks the equality without type conversion.
``` js 
alert( null === undefined ); // false
alert( null == undefined ); // true

// for math and other comparisons except equality
// null becomes 0 and undefined become NaN

alert( null > 0 );  // (1) false
alert( null == 0 ); // (2) false
alert( null >= 0 ); // (3) true

// null only == with undefined by convention. whereas during comparison operators, it is converted to Number (same with undefined)


```


#### More on Operators

- **Terenary operator**  - (boolean) ? (expression) : (expression)
- statement can be of return type or it can be of executional.

- **Logical operator** - Include `||` , `&&` , `!`,`??`
- returns values instead of booleans. 
- Logical OR returns **first truthy** value or **last falsy** one.
- Logical AND returns **first falsy** value or **last truthy** one.
- Logical NOT returns boolean equivalent. 
- `!!` can be used to convert value into its boolean. 
- The precedence of AND `&&` operator is higher than OR `||`.
 
- In a multiple OR statement, evaluation is done from left to right , first truthy value is returned and all other are ignored for evaluation or In case of all being false, last falsey is returned. 

``` js 
alert( firstName || lastName || nickName || "Anonymous"); // SuperCoder

// short circuit evaluation
true || alert("not printed");
false || alert("printed");

// examples
alert( alert(1) || 2 || alert(3) ); // first 1, then 2.
alert( alert(1) && alert(2) ); // 1 and undefined in alert 
alert( null || 2 && 3 || 4 ); //3
```

-  **Nullish coalescing operator** - `??` returns the first argument if it’s not `null/undefined`. Otherwise, the second one.
- `||` checks for first falsy value whereas `??` checks for first defined value; 
``` js 
height = 0 
height || 100; // return 100
height ?? 100; // return 0 
```

#### Loops 

 - break/continue support labels before the loop. A label is the only way for break/continue to escape a nested loop to go to an outer one.
