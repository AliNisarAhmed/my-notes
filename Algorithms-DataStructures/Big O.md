# Asymtotic notation

- Purpose of Asymptotic notation: suppress constant factors and lower-order terms

## Big O

Big O notation is used in Computer Science to describe the time complexity (in terms of number of operations) of an algorithm. The best algorithms will execute the fastest and have the simplest complexity.

Some of the most common are O(n), O(log n), )(n log n), O(n<sup>2</sup>) & O(2<sup>n</sup>).

big O describes an upper bound on time, while big Omega describes a lower bound.

---

Suppose you’ve got a routine that takes one second to process 100 records. How long will it take to process 1,000?

- If your code is `O(1)`, then it will still take one second.
- If it’s `O(log n)`, then you’ll probably be waiting about three seconds.
- `O(n)` will show a linear increase to ten seconds.
- while an `O(n log n)` will take some 33 seconds.
- If you’re unlucky enough to have an `O(n^2)` routine, then sit back for 100 seconds while it does its stuff.
- And if you’re using an exponential algorithm `O(2^n)`, you might want to make a cup of coffee—your routine should finish in about 10^263 years. Let us know how the universe ends.

---

Example: For Quick Sort

**Best Case**: If all elements are equal, then QS will traverse throug the array at least once, hence O(n).

**Worst Case**, if the pivot is always the biggest element in the sub-array, then QS will traverse through the list twice (it will divide the list into sub list of lengths 0 and n), hence big O would be O(n<sup>2</sup>).

**Expected Case** Usually these extreme cases wont happen, so we can expect a runtime of O(n log n).

---

**_What is the relationship between best/worst/expected case and big 0/theta/omega?_**

It's easy for candidates to muddle these concepts (probably because both have some concepts of"higher', "lower" and "exactly right"), but there is no particular relationship between the concepts.
Best, worst, and expected cases describe the big O (or big theta) time for particular inputs or scenarios.
Big 0, big omega, and big theta describe the upper, lower, and tight bounds for the runtime.

---

### Space Complexity

- Time is not the only thing that matters in an algorithm. We might also care about the amount of memory, or space-required by an algorithm.
- Space complexity is a parallel concept to time complexity. If we need to create an array of size n, this will require 0( n) space. If we need a two-dimensional array of size nxn, this will require O( n 2 ) space.

- When we talka about space comlexity, we usually mean **auxilliary space complexity**, it refes to space required by the algorithm, not including the space taken up by the inputs.
- primitives are usually `O(1)` in space complexity, while strings and reference types are `O(n)`.

- In Space complexity, `O(1)` means that a function, no matter the size of the input array, always needs a constant amount of space to perform the task. e.g.

      	```javascript
      		function logUpTo10(n) {
      			for (let i = 0; i <= n; i++) {
      				console.log(i);
      			}
      		}
      	```

- The function below is `O(n)`, because it created an extra array, whose size varies proportionally in terms of the input array

      	```javascript
      		function onlyElementsAtEvenIndex(array) {
      		var newArray = Array(Math.ceil(array.length / 2));
      		for (var i = 0; i < array.length; i++) {
      			if (i % 2 === 0) {
      					newArray[i / 2] = array[i];
      			}
      		}
      			return newArray;
      		}
      	```

---

It is very possible for O(N) code to run faster than O(1) code for specific inputs. Big O just describes the rate of increase.
For this reason, we drop the constants in runtime. An algorithm that one might have described as O(2N) is actually O(N).

---

#### Drop the Contants

- It is very possible for O(N) code to run faster than 0( 1) code for specific inputs. Big O just describes the rate of increase.
- For this reason, we drop the constants in runtime. An algorithm that one might have described as O(2N) is actually O(N).

---

#### Drop the non Dominant terms

You should drop the non-dominant terms.

- O(N 2<sup>N</sup>) becomes O(N<sup>2</sup>).
- O(N + log N) becomes O(N).
- O(5\*2<sup>N</sup> + 1000N<sup>100</sup>) becomes O(2<sup>N</sup> ).

---

#### When to Add vs When to Multiply

```js
for (let i = 0; i < A.length; i++) {
	//
}

for (let j = 0; j < B.length; j++) {
	//
}
```

Runtime: O(A + B)

```js
for (let i = 0; i < A.length; i++) {
	for (let j = 0; j < B.length; j++) {
		//
	}
}
```

Runtime: O(A x B)

**In other words:**

- If your algorithm is in the form "do this, then, when you're all done, do that"then you add the runtimes.
- If your algorithm is in the form "do this for each time you do that"then you multiply the runtimes.

---

**When you see a problem where the number of elements in the
problem space gets halved each time, that will likely be a 0( log N) runtime.**

---

#### Runtime for Recursive Functions

Try to remember this pattern. When you have a recursive function that makes multiple calls, the runtime will often (but not always) look like O(branches<sup>depth</sup>), where branches is the number of times each recursive call branches.

So, for

```js
function f(n) {
	if (n <= 1) {
		return 1;
	}
	return f(n - 1) + f(n - 1);
}
```

Runtime is O (2 <sup>n</sup>)

The space complexity of this algorithm will be O(N). Although we have 0(2 N ) nodes in the tree total, only O(N) exist at any given time. Therefore, we would only need to have O(N) memory available.

---

#### Bases of `log` and exponents

Therefore, if we want to convert log 2 p to log 10 , we just do this:

log<sub>10</sub>p = log<sub>2</sub>p / log<sub>2</sub>10

**Takeaway: Logs of different bases are only off by a constant factor. For this reason, we largely ignore what the base of a log within a big O expression. It doesn't matter since we drop constants anyway.**

- However, this does not apply to exponents. The base of an
  exponent does matter.
  - Compare 2^n and 8^n. If you expand 8^n, you get (2^3)^n, which equals 2^3n,
  - which equals 2^2n \* 2^n. As you can see, 8^n and 2^n are different by a factor of 2^2n. That is very much not a constant factor!

---

#### Memoization and Exponential Time recursive alogs

- Memoization is very common technique to turn exponential time resursive algos (O(2 ^ n)) to O(n).

---

#### Objects and Arrays

Objects have

- `O(1)` entry, `O(1)` value retrieval given a key
- `Object.keys()`, `Object,values()`, `Object,entries()` are `O(n)`
- `Object.hasOwnProperty()` is `O(1)`.

Arrays have

- O(1) insertion/removal at the end
- O(n) insertion/removal at the start
- O(n) search
- O(1) access
- slice(), splice(), map(), reduce(), filter(), concat() are O(n)
- sorting is O(n log n)

---

#### Singly Linked List

**insertion** - O(1)

_Note_: Insertion and Indexing are considered a separate operation, hence when we talk about insertion is O(1) for linked-lists, we already assume that we are in the middle of iterating though the list. For linked list, indexing is O(n), but oonce we have the index, insertion itself is O(1) since we do not have to move the remaining elements. For arrays, indexing is O(1), but, insertion itself is O(n), since all the elements after the index must be re-indexed.

**Removal** - O(1) assuming that the index is known and node has already been identified
**Searching** - O(n)
**Accessing** - O(n)

---

#### Doubly Linked List

**insertion** - O(1)
**Removal** - O(1)
**Searching** - O(n / 2) === O(n)
**Access** - O(n)

A Web browser's back and forward buttons are implemented with a doubly-linked list.

DLL are better than SLL for finding nodes and can be done in half the time.
However, they do take up more memory considering an additional pointer.

---

#### Stack

- Stacks are a LIFO data structure where the last value in is always the first one out.
- Stacks are used to handle function invocations (the call stack), for operations like undo/redo, and for routing (as in browser history)
- we cant use a linked list directly because for stacks, push and pop is O(1), whereas for linked list pop() is O(n)

Big O of stack

- insertion - `O(1)`
- Removal - `O(1)`
- Searching - `O(n)`
- Access - `O(n)`

---

#### Queues

- FIFO data structure
- Used is: Background tasks, uploading resources, printing / task processing
- Big O - Insertion $O(1)$ - Removal $O(1)$ - Searching $O(N)$ - Access $O(N)$

---

#### Trees

Trees are a non-linear DS that contain a root and child nodes

#### Binary Search Trees

BSTs are more specific version of Binary Trees, where there are only two children of each node, and every node to the left of a parent is less than its value, and every node to the right is greater than its value.

- Insertion - `O(log n)`
- Searching - `O(log n)`

Both above are best and average cases, in the worst case (like a completely lop-sided BST), both get worse to `O(n)`

---

### Tree Traversal

#### Breadth-First Search vs Depth First Search

In BFS, We go through all the children of a node before proceeding to the children of children.

In DFS, We traverse all the way to the bottom of the tree, and then work our way back up.
DFS can be done in 3 orders:

1. Pre-Order - In pre-order, we visit the node first, then we traverse its left, then we traverse its right.
2. Post-Order - In post-order, we traverse the left first, then the right, then the node itself is visited.
3. In-order - in in-order, we traverse the left first, then we visit the node then we traverse the right.

Both BFS and DFS have `O(n)` time complexity, as all the values on the tree must be visited.

If the tree is wide, meaning it has a lot of branches which themselves have a lot of branches, then BFS results in huge space complexity, as a "queue" of elements to be visited has to be maintained, which is a huge memory cost.

If a tree has a lot of depth, then DFS can end up taking alot of space.

###### Use cases

DFS-In-order for Binary Search Tree results in an ordered, sorted list of values.
DFS-Pre-order results in the list of values being in the same order as the tree, meaning, from the values array we can easily re-create the whole array. Hence, pre-order results in easy storage and re-creation of that tree.

---

### Binary Heap

- A Binary Tree where
  - every number is either less than the root i.e. root is the largest element (called max heaps)
  - every nunmber is greater than the root i.e. root is the smallest element (called min heaps)

A binary heap is defined as a binary tree with two additional constraints:

- Shape property: a binary heap is a _complete binary tree_; that is, all levels of the tree, except possibly the last one (deepest) are fully filled, and, if the last level of the tree is not complete, the nodes of that level are filled from left to right.
- Heap property: the key stored in each node is either greater than or equal to (≥) or less than or equal to (≤) the keys in the node's children, according to some total order.
- (Where parent >= children)(>=) are called max-heaps, (where parent <= children) (<=) are called min-heaps.

### Priority Queues

- Min Binary Heaps are used to represent a Priority Queue when lower number means higher priority.

#### Time Complexity of Binary Heaps

- insert: $O(\log n)$
- extractMin/extractMax: $O(\log n)$
- delete a key: $(\log n)$
- getMin/getMax (return the root of the tree): $O(1)$

---

### Hash Tables

- Hash Tables ares used to store Key Value pairs
- Like arrays, but the keys are not ordered
- They are found in all programming languages of the world, as objects (JS), Dictionary (python), Map (JS, Haskell, Java).
- A function that performs the conversion of a key and maps it into a value, for storage inside the Hash table is called the **hash function**.
  - What makes a good hash function?
    - Fast (constant time)
    - Uniform output (does not cluster the outputs at specific indices, but ditributes uniformly)
    - Pure (deterministic)
- **Collisions** - when the output of the hash function is same for two or more keys, meaning that we need to store both keys at one hash index
  - Two strategies to deal with Collisions
    - **Separate Chaining** - at each index in the array (the main array), we store multiple values inside another array or a linked list, allowing us to store multiple keys at the same index.
    - **Linear Probing** - when we find a collision, we search through the main array to find the next empty slot, and insert the duplicate KVP there

### Time Complexity

- Insert: $O(1)$
- Deletion: $O(1)$
- Access: $O(1)$

However, a bad hash function, which does not distrbiute the KVPs uniformly (e.g. one that accumulates all the values at one index), may result in degradation of above.
