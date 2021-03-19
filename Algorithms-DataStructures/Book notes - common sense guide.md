# Algo and DS

## Arrays 

When the computer reads a value at a particular index of an array, it can jump straight to that index because

1) Random Access: A computer can jump to any memory address in one step.
2) Whenever a computer allocates an array, it also makes note at which memory addess the array begins.
3) When allocating an array, the computer keeps track of the array's size.

##### Read
Array Read, therefore by points (1) and (2), is $O(1)$ 

##### Search
Array Search is $O(n)$, coz it takes $n$ steps.

##### Insert 
Inserting at the end of the array is $O(1)$, unless the array size limit is reached, then the computer needs to copy the array over to another memory location.

For inserting in middle/start, first a space for the new item needs to be created by shifting remaining items one space, and then an insert is performed, hence $N+1$ steps.

In general, insertion is $O(n)$ 

##### Deletion
Same goes for deletion, which is $O(1)$ at the end, and $O(n)$ in middle/start.

## Ordered Array

##### Read
Still $O(1)$

##### Insert 
it is $O(n)$ 

The total number of steps is $N+2$. 
    - Since we must maintain order, we do an initial search (traversing the array from left to right) to find the proper place of the new item
        - regular array insert does not require a search at all.
    - Having found the place, we need to shift by 1 the remaining items.
    - and then insert

Insertion at the end and beginning take similar steps (traversing from L to R)
    - Because less comparisons when at the beginning, but more shifting
    - More comparisons when at the end, but less shifting

##### Search 
Linear search will be $O(n)$ still, but using Binary Search, an ordered array has search of $O(\log n)$.

Binary Search takes $\lceil log_2 n \rceil$ steps, thus 200-size array will take 8 steps.
    - Each time we double the amount of data, we add only ONE step

*** 

## Hash Tables

- Hash tables give us $O(1)$ reads and insertions.
- The process of taking characters and converting them to numbers is known as _hashing_, and a function that does that is called a _hashing function_
- One classic approach for handling collisions is known as _separate chaining_, when a collision occurs, instead of placing a single value in the cell, it places in it a reference to an array, or a linked list.
- Because of the above, for a poorly chosen hash function which leads to many collisions, the worst-case performance for a hash table lookup is $O(n)$.
- A Hash table's efficiency depends on
    - How much data we're storing in the hash table
    - How many cells are available in the hash table
    - Which hash function we're using
    
- A good hash function is one that distributes its data across _all_ available cells, which leads to fewer collisions.
- However, while avoiding collisions is important, we have to balance that with avoiding memory hogging as well (e.g. reserving 1000 cells for 5-10 pieces of data)
- A good hash table _strikes a balance_ of avoiding collisions while not consuming lots of memory.

**Load Factor**
- Computer Scientist's Rule of thumb: For every 7 data elements stored in a hash table, it should have 10 cells.
- This factor (7 elements / 10 cells) is called the load factor.

*** 

## Stacks and Queues

Stack and Queues are an example of **Abstract Data Types** - Data Structure not built into the language, but that can be constructed from primitive data structures, like arrays.

Stack and Queues are also an example of **Constained Data Structure**, that means, an array can do whatever a stack can do.
Working with contrained DS offers many advantages
    - we can prevent potential bugs
        - e.g. programmer can remove an item from the middle of the array, causing bugs, but in stacks, that option is simply not available.
    - they give us a new mental model for tackling problems.
        - e.g Stacks give us LIFO, Queues give us FIFO
    

### Stacks

Stacks have the following three constraints
- Data can be inserted only at the end of a stack. (push)
- Data can be deleted onlu from the end of the stack. (pull)
- Only the last element of a stack can be read. (peak)

Example usage 
- To find out if opening and closing braces match
- Browser back button and navigation
- function calls in a programming language

### Queues

Queues implement FIFO.

Queues have the following three constraints
- Data can be inserted only at the end of the queue (enqueue) (similar to stacks)
- Data can be deleted only from the front of the queue (dequeue) (opposite of stack)
- Only the element _at the front_ of the queue can be read (OPPOSITE of stack)
  
Queue usage
- printing jobs
- background workers in web applications
