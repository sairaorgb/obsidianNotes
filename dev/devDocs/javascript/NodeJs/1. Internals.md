#### Node Internals

- Node JS is a js runtime that lets you run js outside the web browser.
- Node js comes in with many pre built modules to cator various needs and there is a package manager called npm to install and manage many other third party packages.
``` bash
node app.js
node app.js arg1 arg2
node --watch app.js             # restarts on file changes
node                            # starts REPL (read eval print loop)
```
``` js
process.argv[0]                 // node path
process.argv[1]                 // current file path
process.argv[2]                 // arg1 passed with bash command
process.argv[3]                 // arg2 passed with bash command
```
- JIT compilation , hidden classes , garbage collection and inline caching make v8 faster. 
##### Event loop

- primary components include `v8 engine` that runs js and keeps microtask queue , `libuv` that is a c library that drives event loop, async i/o and a thread pool. `Node bindings` are js apis that provide libuv + system calls.
- In browser , contrary to this chromium manages the event loop and thread pool along with other components to include renders in middle of event loop and all.

- Node asks v8 to drain its microqueue with process.nextTick having priority after which libuv phases get scheduled and microtasks injected btw ticks ( libuv phase loops).
  -  Synchronous code runs first
  -  Async calls get delegated
  -  Call stack gets emptied
  -  Micro task queue in v8
  -  Macro queue/ loop phases 

Libuv phases
- Timers phase - executes callbacks scheduled by setTimeOut and setInterval
- pending callbacks phase - executes i/o callbacks deferred from previous loop
- idle/prepare phase - internal stuff for libuv
- poll phase - 
- check phase - setImmediate callbacks
- close callbacks phase