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