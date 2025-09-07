
## Core Idea

- **Synchronous (blocking)**: Code runs line-by-line, each task must finish before the next starts.
- **Asynchronous (non-blocking)**: Tasks can be deferred, allowing other code to run while waiting (e.g. I/O, timers, network).

JS is **single-threaded** (only one call stack), but async behavior is achieved via the event loop + Web APIs (browser/Node).

##### Terminology

- **Call Stack**: Where JS executes functions (LIFO).
- **Web APIs**: Browser/Node APIs that handle async tasks (e.g. setTimeout, fetch, DOM events, fs in Node).
- **Callback Queue / Task Queue**: Holds completed async tasks waiting to re-enter call stack.
- **Microtask Queue**: Holds promises & mutation observers (higher priority than task queue).
- **Event Loop**: The traffic cop—moves tasks from queues into call stack when it’s free.
##### Execution Flow (Event Loop in action)

1. JS starts with synchronous code → pushed onto call stack.
2. Async calls are delegated to **Web APIs**.
3. Once done, results (callbacks/promises) move into queues.
4. **Event loop** checks: if call stack is empty → takes tasks (microtasks first) → executes them.
##### Key Points

- JS hands the callback to **Web APIs** (e.g. `setTimeout`, `fetch`).
- After task completion → callback placed in **task queue** → **event loop** pushes it to call stack when free.
- **Non-blocking I/O** is why Node.js scales.
- **Microtasks (Promises)** always flush before macrotasks (setTimeout).
- Long-running sync tasks block everything (UI freeze).
- Always handle errors (`try/catch` for async/await, `.catch()` for promises).

## Buzzwords to Remember

- **Concurrency model**: JS concurrency managed by event loop, not threads. 
- **Non-blocking**: Async tasks don’t hold up execution.
- **Deferred execution**: Results delivered later, not immediately. 


## Async Mechanisms

### 1. Callbacks

- A **callback** is a function **passed as an argument** to another function and executed later, often after an async operation completes.
- Key idea: pass **function reference**, not the result of execution.
- Can lead to **Callback Hell** (deep nesting).

``` js
function greet(name, callback){
	console.log("Hello " + name);   
	callback(); 
} 
greet("Sairaorg", () => console.log("Bye!"));

[1, 2, 3].forEach(num => console.log(num));   // synchronous callback
setTimeout(() => greet("Sairaorg"), 1000);    // asynchronous callback
or use `.bind`: setTimeout(greet.bind(null, "Sairaorg"), 1000)

doStep1(res1 => {
	doStep2(res2 => {     
		doStep3(res3 => 
			console.log("Done!")); 
		}); 
	});
});
```
**Error first callbacks in Node js**
```js 
function readFileFake(filename, callback) {
  if (filename !== "exists.txt") {
    callback(new Error("File not found"), null);
  } else {
    callback(null, "file contents here");
  }
}

readFileFake("exists.txt", (err, data) => {
  if (err) {
    console.error("Error:", err);
  } else {
    console.log("Data:", data);
  }
});
```
### 2. Promises

- Object representing a value that may be available now, later, or never.
- States: **pending → fulfilled/rejected**.
- Promise takes executor function as argument with resolve,reject callbacks as function args  which return a value that is taken as argument for then,catch handlers.
- result of a promise is a package of `state of promise` and `return value from resolve/reject`.

1. **creating promise**
``` js
const p = new Promise((resolve, reject) => {
  // Do some async work
  if (/* success */) { resolve("value"); }     // fulfills the promise 
  else { reject("error"); }     // rejects the promise
});
```
- promise is in pending state and moves to fulfilled/rejected upon executing it. 

2. **consuming promise**
```js
p.then(
  value => console.log("fulfilled with", value),
  error => console.error("rejected with", error)
);

p.then( value => console.log("fulfilled with", value )
  .catch( error => console.error("rejected with", error);
  
p.finally(() => console.log("done, success or fail")); 
```
- resolve provides value as arg to first callback . reject executes error callback.
- finally runs regardless of success/failure. Doesnt recieve any arugument from p.

3. **promise chaining**
``` js
p.then(val => val * 2)
 .then(val => `Value: ${val}`)
 .catch(err => console.error(err));
```
 - Each `then` returns a new promise. return values are passed down the chain. Throwing inside turns into rejection.
 
 4. **static helpers**
 ``` js
// utility methods
Promise.resolve(42);      // immediately fulfilled
Promise.reject("fail");   // immediately rejected

// static methods
Promise.all([p1, p2]);    // waits for ALL, rejects if any fail
Promise.race([p1, p2]);   // settles with first to finish
Promise.allSettled([p1]); // waits for ALL, returns results {status,value|reason}
Promise.any([p1, p2]);    // fulfills with first success, ignores rejections
 ```
 5. **Async/await render**
 ``` js
async function demo() {
try {
   const val = await p;   // pauses until p resolves
   console.log(val);
  }catch (err) {
   console.error(err);
  }
}
```

- util.promisfy can be used to render callback based fns into promises

``` js
// promise inside then is resolved and passed down
Promise.resolve(5)
  .then(val => {
    return new Promise(resolve => {
      setTimeout(() => resolve(val * 2), 1000);
    });
  })
  .then(next => {
    console.log(next); // 10 (after 1s)
  });
```
### 3. Async / Await

- async = wraps return in a promise ( promise creation )
- await = unwraps a promise’s value (or throws its error). ( promise consumption ) 
- `await` pauses until promise resolves/rejects. 
- since async functions technically return promises, then catch handlers can be attached to execute them instead of await.

``` js
async function getData() {   
	try {     
		const data = await fetchData();     
		console.log(data);   
	} 
	catch (err) {     
		console.error(err);   
	} 
} 

async function foo() {
  return 42;            // rendered to promise.resolve(42)
}

```

- Try-catch blocks 
``` js
async function baz() {
  try {
    const data = await fetch("bad-url");
    console.log(data);
  } catch (err) {
    console.error("caught:", err);
  }
}

```




