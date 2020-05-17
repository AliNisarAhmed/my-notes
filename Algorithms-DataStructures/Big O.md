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
