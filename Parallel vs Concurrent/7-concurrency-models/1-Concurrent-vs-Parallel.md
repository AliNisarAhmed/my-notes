
## Concurrent vs Parallel

A concurrent program has multiple logical *threads of control*. 
- These threads may or may not run in parallel

A parallel program potentially runs more quickly that a sequential program by executing different parts of the computation simultaneously.
- It may or may not have more than 1 logical thread of control

An alternative way of thinking about this is that:
- *concurrency* is an aspect of the problem domain—your program needs to handle multiple simultaneous (or near-simultaneous) events. 
- *Parallelism*, by contrast, is an aspect of the solution domain—you want to make your program faster by processing different portions of the problem in parallel.


As Rob Pike puts it:
> Concurrency is about dealing with lots of things at once.
   Parallelism is about doing lots of things at once.


Another example:
- A teacher doing multitasking (listening to a child reading, break off to calm down a rowdy class, or answer a question. and then back to the child)
- however, if she has an assistant, then these things can run in parallel


## Non-deterministic


This is unfortunate because concurrent programs are often nondeterministic - they will give different results depending on the precise timing of events. 
- If you’re working on a genuinely concurrent problem, nondeterminism is natural and to be expected. 

Parallelism, by contrast, doesn’t necessarily imply nondeterminism - doubling every number in an array doesn’t (or at least, shouldn’t) become nondeterministic just because you double half the numbers on one core and half on another. 
- Languages with explicit support for parallelism allow you to write parallel code without introducing the specter of nondeterminism.

