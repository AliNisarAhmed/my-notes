# Asynchronous JS

## Problems with callbacks 

1. Callback Hell 
    - call back hell does not only refer to the "pyramid of doom" style indentation that it engenders, it refers to the "criss-cross" nature of code execution when a program is written with callbacks. Our sequential brain is just not adept to deal with such program flows and thus mistakes/bugs become likely.
2. Inversion of Control (and Trust issues stemming from that)
    - Call the callback too early
    - Call the callback too late (or never)
    - Call the callback too few or too many times 
    - Fail to pass along any necessary environment/params to your callback
    - Swallow any errors/exceptions that may happen.

Promises resolve the above issues: 
1. Callback hell does not happen as `async await` makes the async program pretty linear to read
2. Inversion of Control 
    - Calling too early: Not possible in Promise since even an immediately fulfilled promise cannot be observed synchronously. The callback provided to `.then`, even if the promise is fulfilled, will always be called asynchronously.
    - Calling too late: Similar to the previous point, a Promise's `then(..)` registered observation callbacks are automatically scheduled when either `resolve(..)` or `reject(..)` are called by the Promise creation capability. Those scheduled callbacks will predictably be fired at the next asynchronous moment
        
        It's not possible for synchronous observation, so it's not possible for a synchronous chain of tasks to run in such a way to in effect "delay" another callback from happening as expected. That is, when a Promise is resolved, all `then(..)` registered callbacks on it will be called, in order, immediately at the next asynchronous opportunity (again, see "Jobs" in Chapter 1), and nothing that happens inside of one of those callbacks can affect/delay the calling of the other callbacks.
    - nothing (not even a JS error) can prevent a Promise from notifying you of its resolution (if it's resolved). If you register both fulfillment and rejection callbacks for a Promise, and the Promise gets resolved, one of the two callbacks will always be called.

        Of course, if your callbacks themselves have JS errors, you may not see the outcome you expect, but the callback will in fact have been called.
        
    - Promises are defined so that they can only be resolved once. If for some reason the Promise creation code tries to call `resolve(..)` or `reject(..)` multiple times, or tries to call both, the Promise will accept only the first resolution, and will silently ignore any subsequent attempts.

        Because a Promise can only be resolved once, any `then(..)` registered callbacks will only ever be called once (each).
        
        Of course, if you register the same callback more than once, (e.g., `p.then(f); p.then(f);`), it'll be called as many times as it was registered. The guarantee that a response function is called only once does not prevent you from shooting yourself in the foot.
        
    - Promises can have, at most, one resolution value (fulfillment or rejection).

        If you don't explicitly resolve with a value either way, the value is `undefined`, as is typical in JS. But whatever the value, it will always be passed to all registered (and appropriate: fulfillment or rejection) callbacks, either _now_ or in the future.

#### Execution Context

is an abstract concept of an anvironment where the JS Code is evaluated and executed. Whenever any code is run in JS, it's run inside an Execution Context

So, a function executes inside a Function EC, while global code executes inside a global EC.

#### Call Stack

The call stack as its name implies is a stack with a LIFO (Last in, First out) structure, which is used to store all the execution context created during the code execution.

---

The **Event loop**, **the web APIs** and the **message queue/task queue** are not part of the JavaScript engine, it’s a part of browser’s JavaScript runtime environment or Nodejs JavaScript runtime environment (in case of Nodejs). In Nodejs, the web APIs are replaced by the C/C++ APIs.

---

#### The Event Loop & Message / Task Queue

Event Loop is a Queue (FIFO). The job of the Event loop is to look into the call stack and determine if the call stack is empty or not.

If the call stack is not empty, all code in the call stack is executed first before any code in the Message Queue is touched, this is called **Starving the Event Loop**.

If the call stack is empty, it looks into the message queue to see if there’s any pending callback waiting to be executed and if there is, it is executed.

---

**The Message queue** also contains the callbacks from the DOM events such as click events and keyboard events.

In case of DOM events, the event listener sits in the web APIs environment waiting for a certain event (click event in this case) to happen, and when that event happens, then the callback function is placed in the message queue waiting to be executed.

---

#### Job Queue / Microtask Queue

ES6 introduced the concept of job queue/micro-task queue which is used by Promises in JavaScript. The difference between the message queue and the job queue is that the job queue has a higher priority than the message queue, which means that promise jobs inside the job queue/ micro-task queue will be executed before the callbacks inside the message queue.

---

#### Promises

Promises are objects representing the eventual completion (or failure) of an asynchronous operation, and its resulting value.

A Promise is in one of these states:

- **pending**: initial state, neither fulfilled nor rejected.
- **fulfilled**: meaning that the operation completed successfully.
- **rejected**: meaning that the operation failed.

---

#### Micro-Task Queue vs Task Queue vs JS Stack

The order of execution for microtask queues different depending on if the JS stack is empty.

Micro-task queues have the highest priority, but if the JS stack is not empty, then even
the microtask queue is defered.

```javascript
  button.addEventListener('click', () => {
    Promise.resolve().then(() => console.log('Microtask 1));
    console.log('Listener 1);
  });

  button.addEventListener('click', () => {
    Promise.resolve().then(() => console.log('Microtask 2));
    console.log('Listener 2);
  });
```

- The order of execution and the appearance of `console.log` in the above snippet would be
  - Listener1 -> Microtask1 -> Listener2 -> Microtask2
  - This happens because when the listener one finishes, there is nothing in JS call stack, but now there is a task in Micro-Task queue, which takes over and nothing else is allowed to be executed till the MQ Task is empty.

But now consider

```javascript
  button.addEventListener('click', () => {
    Promise.resolve().then(() => console.log('Microtask 1));
    console.log('Listener 1);
  });

  button.addEventListener('click', () => {
    Promise.resolve().then(() => console.log('Microtask 2));
    console.log('Listener 2);
  });

  button.click();
```

- Same code as above, but now the click is being done by JS.
- Now, the JS engine reaches the `.click()` line, starts executing it.
- It print Listener1, but `.click()` script is not done yet, it is still in the stack.
- So the engine cannot move to the MT Queue, it must continue executing the current JS stack.
- Remember, JS stack (which is synchronous code), has the highest priority.
- after jS stack is empty, the MT Queue is then executed.
- So the order here would be Listener1 -> Listener2 -> Microtask1 -> MicroTask2

---
