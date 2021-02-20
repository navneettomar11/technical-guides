# Worker Threads
To understand Workers first, it necessary to understand how Node.js is structured.
When a Node.js process is launched, it runs:
- One process: a process is a global object that cane be  accessed anywhere and has information about what's being executed at a time.
- One thread: being single-threaded means that only one set of instructions is executed at a time in a given process.
- One event loop: this is one of the most important aspects to understand about Node. It's what allows Node to be asynchronous and have non-blocking I/O - despite the fact that Javascript is single threaded - by offloading operations to the system kernel whenever possible through callbacks, promise and async/await.
- One JS Engine instance: this a computer program that executes Javascript code.
- One Node.js instance: the computer program that executes Node.js code.

In other words, Node runs on a single thread, and there is just one process happening at a time in event loop. Once code, one executuon(the code is not executed in parallel). This is very useful because it simplifies how you use Javascript without worrying about concurrency issues.

The reason it was built with the approach is that Javascript was initially created for client-side interactions (like web page interactions, or form validation) -- nothing that required the complexity of multithreading.

But, as with all things is a downside: if you have CPU-intensive code, like complex calucation in a large dataset taking place in-memory, it can block other processes from being executed. Similary, if you are making a request to a server that has CPU-intensive code, that code block the event loop and prevent other requests of being handled.

A function is considered "blocking" if the main event loop must wait until it has finished executing the next command. A "Non-blocking" function will allow the main even loop to continue as soon as it begins and typically alerts the main loop once it has finsihed by calling a "callback".

> The golden rule: don't block event loop, try keep it running it and pay attention and avoid anything that could block the thread like synchronous network calls or infinite loop.

It's important to differentiate between CPU operations and I/O operations. As mentioned earilier, the code of Node.js is NOT executed in parellel. Only I/O operations are run in parallel, because they are exeuted asychronously.

So Worker Thread will not help much with I/O-intensive work because asynchronous I/O operation are more efficient than Worker can be. The main goal of Workers is to improve the performance on CPU-intensive operations not I/O operations.

## Some Solutions
Furthermore, there are already solutions for CPU intensive operations: multiple processes (like cluster API) that make sure that the CPU is optimally used.

This approach is advantageous because it allows isolation of processes, so if something goes wrong in on process, it doesn't affect the others. They also have stability and identical APIs. However, this means sacrificing shared memory, and the communication of data must be via JSON.

### Javascript and Node.js will never have threads, this why
So, people might think that adding a new module in Node.js core will allow us to create and sync threads, thus solving the problem of CPU-intensive operations.

Well, no really. If threads are added, the nature of the language itself will change. It's not possible to add threads as a new set of available classes or functions. In language that supports multithreading (like Java) keywords such as "synchronized" help to enable multiple thread to sync.

Also, some numeric types are not atomic, meaning that if you don't synchronize them, you could end up having two threads changing the value of a variable and resulting that after both threads have accessed it, the variable has a few bytes changed by one thread and few bytes changed by the other thread and thus, not resulting in any value. For example, in the simple operation of 0.1 + 0.2 has 17 decimals in Javascript(the maximum number of decimals).

> var x = 0.1 + 0.2; // x will be 0.30000000000000004

By floating point arithmetic is not always 100% accurate. So if not synchronized, one decimal may get changed using Workers, resulting in non-identical numbers.

### The best solution
The best solution for CPU performance is Worker Threads. Browsers have had the concept of Workers for a long time.
Instead of having
- One process
- One thread 
- One event loop
- One JS Engine Instance
- One Node.js Instance

Worker threads have:
- One process
- Multiple threads
- One event loop per thread
- One JS Engine instance per thread
- One Node.js instance per thread

As we can see in the following image:
![Worker thread](https://images.ctfassets.net/hspc7zpa5cvq/20h5efXHT4bQbuf44mdq2H/a40944191d031217a9169b17a8ef35d6/worker-diagram_2x__1_.jpg)

The `worker_threads` module enables the use of threads that execute  Javascript in parallel. To access it:

```javascript
const worker = require('worker_threads');
```
Worker Threads have been available since Node.js 10, but are still in the experimental phase.

What is ideal, is to have multiple Node.js instances insided the same process. With Worker threads, a thread can end at some point and it's not necessarily the end of the parent process. It's not a good pratice for resources that were allocated by a Worker to hang around when the Worker is gone - that's a memory leak, and we don't want that. We want to embed Node.js into itself, give Node.js the ability to create a new thread and then create a new Node.js instance inside that thread; essentially running independent threads inside the same process.

