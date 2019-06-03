# Asynchronous JS

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
