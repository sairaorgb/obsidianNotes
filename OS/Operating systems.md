Operating system is a body of software that is responsible for making it easy to run programs (even allowing you to seemingly run many at the same time), allowing programs to share memory, enabling programs to interact with devices, and other fun stuff like that.

- OS provides a standard library to applications.
- OS is sometimes known as a resource manager.

Virtualizing CPU and memory
- Turning a single CPU (or a small set of them) into a seemingly infinite number of CPU's and thus allowing many programs to seemingly run at once is what we call virtualizing the CPU.
- Each process accesses its own private virtual address space (sometimes just called its address space), which the OS somehow maps onto the physical memory of the machine

- Both the CPU and memory are virtualized - it’s like each program has access to its own private copy at a time
- Strange things happen when dealing with concurrency. Load, increment, store back don’t happen atomically (all at once).
- Persistence - storing files in the file system. This is messy, the OS does a lot of work and makes things available through system calls (standard library)
- We have to build abstractions
- We care about time complexity
- And space complexity (on disk and in memory)
- Protection/isolation
- Reliability
- Energy efficiency
- Security
- Mobility (portability)
- System call vs procedure call - system calls elevate permission to the hardware privilege level via a trap/trap handler

### Processes

- A process is a running program
- We use time-sharing to virtualize (space-sharing also exists)
    - This is used in networking
- Low-level part: time-sharing mechanism, a context switch
- High-level part: a scheduling policy
- These should be separate, because modularity
- What makes up a process? (machine state)
    - Memory (Address space)
    - Registers
    - Program counter / instruction pointer
    - Stack pointer & associated frame pointer
    - Persistent storage devices, open files (I/O information)
- Process API
    - Create
    - Destroy
    - Wait
    - Miscellaneous Control
    - Status
- Loading copies code & static data, initializes heap & stack
- Process states
    - Running
    - Ready
    - Blocked
- Processes can be scheduled & descheduled. What should the scheduling policy be after one process gets blocked & another starts running?

### Process API

**Fork system call**
- when OS encounters a fork call , new resources similar to the original process are created and then made to run independently. fork call in parent process returns pid of child but 0 in child.  execution chance might be given to either parent or child.
**Wait system call**
- wait system call can be called by parent to stop its execution til the child of it completes execution. 
**Exec call**
- exec call when encountered in a process, transforms the resources of current process into the resources of new process called in exec. The older process gets erased from memory.
- `pipe` is also a system call!

``` c
1 #include <stdio.h>
2 #include <stdlib.h>
3 #include <unistd.h>
4 #include <string.h>
5 #include <sys/wait.h>
6
7 int
8 main(int argc, char *argv[])
9 {
	printf("hello world (pid:%d)\n", (int) getpid());
11 int rc = fork();
12 if (rc < 0) { // fork failed; exit
13 fprintf(stderr, "fork failed\n");
14 exit(1);
15 } else if (rc == 0) { // child (new process)
16 printf("hello, I am child (pid:%d)\n", (int) getpid());
17 char *myargs[3];
18 myargs[0] = strdup("wc"); // program: "wc" (word count)
19 myargs[1] = strdup("p3.c"); // argument: file to count
20 myargs[2] = NULL; // marks end of array
21 execvp(myargs[0], myargs); // runs word count
22 printf("this shouldn’t print out");
23 } else { // parent goes down this path (main)
24 int wc = wait(NULL);
25 printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
26 rc, wc, (int) getpid());
27 }
28 return 0;
29 }
```

``` bash
prompt> ./p3
hello world (pid:29383)
hello, I am child (pid:29384)
29 107 1030 p3.c
hello, I am parent of 29384 (wc:29384) (pid:29383)
prompt>
```

