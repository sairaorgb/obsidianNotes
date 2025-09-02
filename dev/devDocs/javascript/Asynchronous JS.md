
## Core Idea

- **Synchronous (blocking)**: Code runs line-by-line, each task must finish before the next starts.
- **Asynchronous (non-blocking)**: Tasks can be deferred, allowing other code to run while waiting (e.g. I/O, timers, network).
JS is **single-threaded** (only one call stack), but async behavior is achieved via the **event loop** + **Web APIs** (browser/Node).

## Terminology

- **Call Stack**: Where JS executes functions (LIFO).
- **Web APIs**: Browser/Node APIs that handle async tasks (e.g. setTimeout, fetch, DOM events, fs in Node).
- **Callback Queue / Task Queue**: Holds completed async tasks waiting to re-enter call stack.
- **Microtask Queue**: Holds promises & mutation observers (higher priority than task queue).
- **Event Loop**: The traffic cop—moves tasks from queues into call stack when it’s free.

## Async Mechanisms

### 1. Callbacks

- Functions passed into other functions to be executed later.
- Can lead to **Callback Hell** (deep nesting).

``` js
doStep1(res1 => {
	doStep2(res2 => {     
		doStep3(res3 => 
			console.log("Done!")); 
		}); 
	});
});
 ```

### 2. Promises

- Object representing a value that may be available now, later, or never.
- States: **pending → fulfilled/rejected**.
- `.then()` for fulfillment, `.catch()` for rejection, `.finally()` for cleanup.

``` js
fetchData()
	.then(data => console.log(data))   
	.catch(err => console.error(err));
```

### 3. Async / Await

- Syntactic sugar over promises. 
- `await` pauses inside async function until promise resolves/rejects.

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
```

## Execution Flow (Event Loop in action)

1. JS starts with synchronous code → pushed onto call stack.
2. Async calls are delegated to **Web APIs**.
3. Once done, results (callbacks/promises) move into queues.
4. **Event loop** checks: if call stack is empty → takes tasks (microtasks first) → executes them.

## Key Points

- **Non-blocking I/O** is why Node.js scales.
- **Microtasks (Promises)** always flush before macrotasks (setTimeout).
- Long-running sync tasks block everything (UI freeze).
- Always handle errors (`try/catch` for async/await, `.catch()` for promises).

## Buzzwords to Remember

- **Concurrency model**: JS concurrency managed by event loop, not threads. 
- **Non-blocking**: Async tasks don’t hold up execution.
- **Deferred execution**: Results delivered later, not immediately. 


# Callbacks in JavaScript

## Definition

- A **callback** is a function **passed as an argument** to another function and executed later, often after an async operation completes.
- Key idea: pass **function reference**, not the result of execution.
## Syntax

``` js
function greet(name, callback){
	console.log("Hello " + name);   
	callback(); 
} 
greet("Sairaorg", () => console.log("Bye!"));`
setTimeout(() => greet("Sairaorg"), 1000);
or use `.bind`: `setTimeout(greet.bind(null, "Sairaorg"), 1000)
```



## Async Usage

- JS hands the callback to **Web APIs** (e.g. `setTimeout`, `fetch`).
- After task completion → callback placed in **task queue** → **event loop** pushes it to call stack when free.
## Patterns

- **Continuation style**: subsequent work runs _inside_ the callback.
- **Error-first callbacks (Node.js)**: `(err, result) => { ... }`.

## Pitfalls

- **Callback Hell**: deep nesting for dependent async tasks.
- Harder error handling and sequencing.
- Motivated the creation of **Promises** and **async/await**.


