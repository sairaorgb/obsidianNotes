
#### Rest vs Spread (`...` in JS)  
 
 **Rest** (`...args`) 
 
 - Collects multiple values into a single array. 
 - Used in function parameters or destructuring.   
```js
function sum(...nums) { 
	return nums.reduce((a,b)=>a+b,0); 
} 
const [first, ...rest] = [1,2,3,4]; // rest = [2,3,4] 
``` 

**Spread** (`...iterable`)

- Expands an iterable/object into individual elements.
- Used in **function calls, array literals, object literals**.
``` js
Math.max(...[1,2,3]); // 3 
const arr = [..."hi"]; // ["h","i"] 
const obj = {...{a:1}, b:2}; // {a:1, b:2}
```

- **Rest = gather** (many → one array).
- **Spread = scatter** (one iterable → many values).
- Works only with **iterables** (spread) and at the **last position** (rest).

 