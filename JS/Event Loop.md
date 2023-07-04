
# Single Threaded nature of JS

Event loop can be thought of as an infinitely running loop that continuously checks to see if there is work to perform.
- When it finds something to do, it begins its task i.e, executes a function in a new call stack
- and once the function is complete, it waits until more work is ready to be performed


Example:

```js
function a() { b(); }
function b() { c(); }
function c() {  / ** / }

function x() { y(); }
function y() { z(); }
function z() { /**/ }

setTimeout(x, 0);
a();
```

The call stack of the above code

![[Pasted image 20230703152106.png]]

The `setTimeout()` function is essentially saying, “Try to run the provided function 0ms from now.” However, the `x()` function doesn’t run _immediately_, as the `a()` call stack is still in progress. It doesn’t even run immediately after the `a()` call stack is complete, either. The event loop takes a nonzero amount of time to check for more work to perform. It also takes time to prepare the new call stack. So, even though `x()` was scheduled to run in 0ms, in practice it may take a few milliseconds before the code runs, a discrepancy that increases as application load increases.

Another thing to keep in mind is that functions can take a long time to run. If the `a()` function took 100ms to run, then the earliest you should expect `x()` to run might be 101ms. Because of this, think of the time argument as the earliest time the function can be called. A function that takes a long time to run is said to _block the event loop_—since the application is stuck processing slow synchronous code, the event loop is temporarily unable to process further tasks.


## Question

If the code in Example below were executed, in what order would you expect the messages to be printed to the screen? And, as a bonus, how much time would you expect to pass before each message is printed?


```js
setTimeout(() => console.log('A', 0));
console.log('B');
setTimeout(() => console.log('C'), 100);
setTimeout(() => console.log('D'), 0);

let i = 0;
while (i < 1_000_000_000) { // assume this takes ~500ms
	let ignore = Math.sqrt(i);
	i++;
}

console.log('E');
```

Answer: B (1) -> E (501) -> A (502) -> D (502) -> C (502)

The first thing that happens is the function to log A is scheduled with a timeout of 0ms. Recall that this doesn’t mean the function will run in 0ms; instead it is scheduled to run as early as 0 milliseconds but after the current stack ends. Next, the log B method is called directly, so it’s the first to print. Then, the log C function is scheduled to run as early as 100ms, and the log D is scheduled to happen as early as 0ms.

Then the application gets busy doing calculations with the while loop, which eats up half a second of CPU time. Once the loop concludes, the final call for log E is made directly and it is now the second to print. The current stack is now complete. At this point, only a single stack has executed.

Once that’s done, the event loop looks for more work to do. It checks the queue and sees that there are three tasks scheduled to happen. The order of items in the queue is based on the provided timer value and the order that the `setTimeout()` calls were made. So, it first processes the log A function. At this point the script has been running for roughly half a second, and it sees that log A is roughly 500ms overdue, and so that function is executed. The next item in the queue is the log D function, which is also roughly 500ms overdue. Finally, the log C function is run and is roughly 400ms overdue.


---

# NodeJS overview

Node.js fully embraces the Continuation-Passing Style (CPS) pattern throughout its internal modules by way of _callbacks_—functions that are passed around and invoked by the event loop once a task is complete. In Node.js parlance, functions that are invoked in the future with a new stack are said to be run _asynchronously_. Conversely, when one function calls another function in the same stack, that code is said to run _synchronously_.

The types of tasks that are long-running are typically I/O tasks. For example, imagine that your application wants to perform two tasks. Task A is to read a file from disk, and Task B is to send an HTTP request to a third-party service. If an operation depends on both of these tasks being performed—an operation such as responding to an incoming HTTP request—the application can perform the operations in parallel, as shown in Figure below. If they couldn’t be performed at the same time—if they had to be run sequentially—then the overall time it takes to respond to the incoming HTTP request would be longer.

![[Pasted image 20230703153417.png]]


***Question arises: How does NodeJS runs two tasks in parallel?

Node.js itself _is_ multithreaded. The lower levels of Node.js are written in C++. This includes third-party tools like _libuv_, which handles operating system abstractions and I/O, as well as V8 (the JavaScript engine) and other third-party modules. The layer above that, the Node.js binding layer, also contains a bit of C++. It’s only the highest layers of Node.js that are written in JavaScript, such as parts of the Node.js APIs that deal directly with objects provided by userland (application code and npm packages)

![[Pasted image 20230703153622.png]]

Internally, libuv maintains a thread pool for managing I/O operations, as well as CPU-heavy operations like `crypto` and `zlib`. This is a pool of finite size where I/O operations are allowed to happen. If the pool only contains four threads, then only four files can be read at the same time. 

Consider Example below where the application attempts to read a file, does some other work, and then deals with the file content. Although the JavaScript code within the application is able to run, a thread within the bowels of Node.js is busy reading the content of the file from disk into memory.

```js
const fs = require('fs');

fs.readFile('/etc/passwd', (err, data) => { // (1)
	if (err) throw err; // (4)

	console.log(data);
})

setImmediate( // (2)
	() => console.log('This runs while the file is being read') // (3)
);
```

1. NodeJS reads file, scheduled by `libuv`
2. NodeJS runs a callback in a new stack. It's scheduled by V8
3. Once the previous stack ends, a new stack is created and prints a message
4. Once the file is done reading, `libuv` passes the result to V8 event loop

Internally, Node.js maintains a list of asynchronous tasks that still need to be completed. This list is used to keep the process running. When a stack completes and the event loop looks for more work to do, if there are no more operations left to keep the process alive, it will exit. That is why a very simple application that does nothing asynchronous is able to exit when the stack ends. Here’s an example of such an application:

```js
console.log('Print, then exit');
```

However, once an asynchronous task has been created, this is enough to keep a process alive, like in this example:

```js
setInterval(() => {
	console.log('Process wil run forever');
}, 1000)
```

There are many Node.js API calls that result in the creation of objects that keep the process alive. As another example of this, when an HTTP server is created, it also keeps the process running forever. A process that closes immediately after an HTTP server is created wouldn’t be very useful.

There is a common pattern in the Node.js APIs where such objects can be configured to no longer keep the process alive. Some of these are more obvious than others. For example, if a listening HTTP server port is closed, then the process may choose to end. Additionally, many of these objects have a pair of methods attached to them, `.unref()` and `.ref()`. The former method is used to tell the object to no longer keep the process alive, whereas the latter does the opposite. Example below demonstrates this happening.

```js
const t1 = setTimeout(() => {}, 1_000_000); // (1)
const t2 = setTimeout(() => {}, 2_000_000); // (2)

t1.unref(); // (3)
clearTimeout(t2); // (4)
```
1. There is now one asynchronous operation keeping Node.js alive. The process should end in 1,000 seconds.
2. There are now two such operations. The process should now end in 2,000 seconds.
3. The _t1_ timer has been unreferenced. Its callback can still run in 1,000 seconds, but it won’t keep the process alive.
4. The _t2_ timer has been cleared and will never run. A side effect of this is that it no longer keeps the process alive. With no remaining asynchronous operations keeping the process alive, the next iteration of the event loop ends the process.


---


# Event Loop

Both the JavaScript that runs in your browser and the JavaScript that runs in Node.js come with an implementation of an event loop. They’re similar in that they both schedule and execute asynchronous tasks in separate stacks. But they’re also different since the event loop used in a browser is optimized to power modern single page applications, while the one in Node.js has been tuned for use in a server. This section covers, at a high level, the event loop used in Node.js. Understanding the basics of the event loop is beneficial because it handles all the scheduling of your application code—and misconceptions can lead to poor performance.

As the name implies, the event loop runs in a loop. The elevator pitch is that it manages a queue of events that are used to trigger callbacks and move the application along. But, as you might expect, the implementation is much more nuanced than that. It executes callbacks when I/O events happen, like a message being received on a socket, a file changing on disk, a `setTimeout()` callback being ready to run, etc.

At a low level, the operating system notifies the program that _something_ has happened. Then, libuv code inside the program springs to life and figures out what to do. If appropriate, the message then bubbles up to code in a Node.js API, and this can finally trigger a callback in application code. The event loop is a way to allow these events in lower level C++ land to cross the boundary and run code in JavaScript.


## Event Loop Phases

The event loop has several different phases to it. Some of these phases don’t deal with application code directly; for example, some might involve running JavaScript code that internal Node.js APIs are concerned about.

![[Pasted image 20230703154735.png]]

Each one of these phases maintains a queue of callbacks that are to be executed. Callbacks are destined for different phases based on how they are used by the application. Here are some details about these phases:

### Poll

The poll phase executes I/O-related callbacks. This is the phase that application code is most likely to execute in. When your main application code starts running, it runs in this phase.

### Check

In this phase, callbacks that are triggered via `setImmediate()` are executed.

### Close

This phase executes callbacks that are triggered via `EventEmitter` `close` events. For example, when a `net.Server` TCP server closes, it emits a `close` event that runs a callback in this phase.

### Timers

Callbacks scheduled using `setTimeout()` and `setInterval()` are executed in this phase.

### Pending

Special system events are run in this phase, like when a `net.Socket` TCP socket throws an `ECONNREFUSED` error.


## MicroTask Queues

To make things a little more complicated, there are also two special _microtask queues_ that can have callbacks added to them while a phase is running. 

The first microtask queue handles callbacks that have been registered using `process.nextTick()`. 

The second microtask queue handles promises that reject or resolve. 

Callbacks in the microtask queues take priority over callbacks in the phase’s normal queue, and callbacks in the next tick microtask queue run before callbacks in the promise microtask queue.

> A “tick” refers to a complete pass through the event loop. Confusingly, `setImmediate()` takes a tick to run, whereas `process.nextTick()` is more immediate, so the two functions deserve a name swap.


When the application starts running, the event loop is also started and the phases are handled one at a time. Node.js adds callbacks to different queues as appropriate while the application runs. When the event loop gets to a phase, it will run all the callbacks in that phase’s queue. Once all the callbacks in a given phase are exhausted, the event loop then
moves on to the next phase. 

If the application runs out of things to do but is waiting for I/O operations to complete, it’ll hang out in the `poll` phase.


## Code Example

```js
const fs = require('fs');

setImmediate(() => console.log(1));
Promise.resolve().then(() => console.log(2));
process.nextTick(() => console.log(3));

fs.readFile(__filename, () => {
	console.log(4);
	setTimeout(() => console.log(5));
	setImmediate(() => console.log(6));
	process.nextTick(() => console.log(7));
});

console.log(8);
```

Answer: 8 -> 3 -> 2 -> 1 -> 4 -> 7 -> 6 -> 5

- The script starts off executing line by line in the `poll` phase. First, the `fs` module is required, and a whole lot of magic happens behind the scenes. 
- Next, the `setImmediate()` call is run, which adds the callback printing 1 to the check queue. 
- Then, the promise resolves, adding callback 2 to the promise microtask queue. 
- `process.nextTick()` runs next, adding callback 3 to the next tick microtask queue. 
- Once that’s done the `fs.readFile()` call tells the Node.js APIs to start reading a file, placing its callback in the poll queue once it’s ready. 
- Finally, log number 8 is called directly and is printed to the screen.
- That’s it for the current stack. Now the two microtask queues are consulted. 
- The next tick microtask queue is always checked first, and callback 3 is called. 
- Since there’s only one callback in the next tick microtask queue, the promise microtask queue is checked next. Here callback 2 is executed. 
- That finishes the two microtask queues, and the current `poll` phase is complete.
- Now the event loop enters the `check` phase. 
- This phase has callback 1 in it, which is then executed. 
- Both the microtask queues are empty at this point, so the `check` phase ends. 
- The `close` phase is checked next but is empty, so the loop continues. 
- The same happens with the `timers` phase and the `pending` phase, and the event loop continues back around to the poll phase.
- Once it’s back in the `poll` phase, the application doesn’t have much else going on, so it basically waits until the file has finished being read. Once that happens the `fs.readFile()` callback is run.
- The number 4 is immediately printed since it’s the first line in the callback. 
- Next, the `setTimeout()` call is made and callback 5 is added to the timers queue. 
- The `setImmediate()` call happens next, adding callback 6 to the check queue. 
- Finally, the `process.nextTick()` call is made, adding callback 7 to the next tick microtask queue. 
- The `poll` queue is now finished, and the microtask queues are again consulted. 
- Callback 7 runs from the next tick queue, the promise queue is consulted and found empty, and the `poll` phase ends.
- Again, the event loop switches to the `check` phase where callback 6 is encountered. The number is printed, the microtask queues are determined to be empty, and the phase ends. 
- The `close` phase is checked again and found empty. 
- Finally the `timers` phase is consulted wherein callback 5 is executed. 
- Once that’s done, the application doesn’t have any more work to do and it exits.


## Async Code example

```javascript
const sleep_st = (t) => new Promise((r) => setTimeout(r, t));
const sleep_im = () => new Promise((r) => setImmediate(r));

(async () => {
	setImmediate(() => console.log(1));
	console.log(2);

	await sleep_st(0);
	setImmediate(() => console.log(3));
	console.log(4);
	await sleep_im();
	setImmediate(() => console.log(5));
	console.log(6);
	await 1;
	setImmediate(() => console.log(7));
	console.log(8);
})();
```

Answer: 2 -> 1 -> 4 -> 3 -> 6 -> 8 -> 5 -> 7

Another way to write above code:
```js
setImmediate(() => console.log(1));
console.log(2);
Promise.resolve().then(() => setTimeout(() => {
	setImmediate(() => console.log(3));
	console.log(4);
	Promise.resolve().then(() => setImmediate(() => {
		setImmediate(() => console.log(5));
		console.log(6);
		Promise.resolve().then(() => {
			setImmediate(() => console.log(7));
			console.log(8);
		});
	}));
}, 0));
```


## Event Loop tips

### Don't starve the event loop

Running too much code in a single stack will stall the event loop and prevent other callbacks from firing. 
- One way to fix this is to break CPU-heavy operations up across multiple stacks. 
- For example, if you need to process 1,000 data records, you might consider breaking it up into 10 batches of 100 records, using `setImmediate()` at the end of each batch to continue processing the next batch. 
- Depending on the situation, it might make more sense to offload processing to a child process.

You should never break up such work using `process.nextTick()`. 
- Doing so will lead to a microtask queue that never empties—your application will be trapped in the same phase forever! 
- Unlike an infinitely recursive function, the code won’t throw a `RangeError`. Instead, it’ll remain a zombie process that eats through CPU. 
- Check out the following for an example of this:

```js
const nt_recursive = () => process.nextTick(nt_recursive);
nt_recursive(); // setInterval below will never return

const si_recursive = () => setImmediate(si_recursive);

setInterval(() => console.log('h1'), 10);
```

In this example, the `setInterval()` represents some asynchronous work that the application performs, such as responding to incoming HTTP requests. 
- Once the `nt_recursive()` function is run, the application ends up with a microtask queue that never empties and the asynchronous work never gets processed. 
- But the alternative version `si_recursive()` does not have the same side effect. 
- Making `setImmediate()` calls within a check phase adds callbacks to the _next_ event loop iteration’s check phase queue, not the current phase’s queue.


### Don't introduce Zalgo

When exposing a method that takes a callback, that callback should always be run asynchronously. 

For example, it’s far too easy to write something like this:

```js
// antipattern
function foo(count, callback) {
	if (count <= 0) {
		return callback(new TypeError('count > 0'))
	}

	myAsyncOperation(count, callback)
}
```

The problem with above code is that the callback is sometimes called synchronously, like when `count` is set to zero, and sometimes asynchronously, like when `count` is set to one. 

Instead, ensure the callback is executed in a new stack, like in this example:

```js
function foo(count, callback) {
	if (count <= 0) {
		return process.nextTick(() => callback(new TypeError('count > 0')))
	}

	myAsyncOperation(count, callback);
}
```

In this case, either using `setImmediate()` or `process.nextTick()` is okay; just make sure you don’t accidentally introduce recursion. With this reworked example, the callback is always run asynchronously. 

Ensuring the callback is run consistently is important because of the following situation:

```js
let bar = false;
foo(3, () => {
	assert(bar);
});
bar = true;
```

This might look a bit contrived, but essentially the problem is that when the callback is sometimes run synchronously and sometimes run asynchronously, the value of `bar` may or may not have been modified. In a real application this can be the difference between accessing a variable that may or may not have been properly initialized.