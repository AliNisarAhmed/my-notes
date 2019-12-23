# Big O - Practice

```js
function foo (array) {
  for (let i = 0; i < array.length; i++) {
    //
  }
  for (let i = 0; i < array.length; i++) {
    //
  }
}
```
<details><summary>Answer:</summary>
O(n) -  two or more serial for loops always result in O(n)
</details>

---
```js
function foo (arr) {
  for (i = 0; i < arr.length; i++) {
    for (i = 0; i < arr.length; i++) {
      //
    }
  }
}
```
<details><summary>Answer:</summary>
O(n<sup>2</sup>)
</details>

---
```js
function foo (arr) {
  for (i = 0; i < arr.length; i++) {
    for (j = i + 1, j < arr.length; j++) { // inner loop starts at i + 1
      //
    }
  }
}
```
<details><summary>Answer:</summary>
O(n^2) <br>
Reasons: <br>
1. first time, inner loop runs (n-1) times, second time (n - 2) times, and so on... <br>
    so total number of steps (n - 1) + (n - 2) + (n - 3) + ... + 2 + 1<br>
    = 1 + 2 + 3 ... + (n - 1)<br>
    = n ( n - 1) / 2<br>
    = n<sup>2</sup> <br>
2. there are total (i, j) pairs, for half of these i is less than j, hence code goes thru the other half, which is N<sup>2</sup>/2.<br>
3. Average Work <br>
    - the outer loop runs N times, the inner one runs (n - 1) + (n - 2) + ... 2 + 1 times, the avg of that is N / 2.<br>
    - since inner loop runs N times, hence total work done is N(N/2) = N<sup>2</sup>/2. 
</details>

---
```js
function foo(arrA, arrB) {
  for (i = 0; i < arrA.length; i++) {
    for (j = 0; j < arrB.length; j++) {
      if (arrA[i] < arrB[j]) {
        console.log(...);
      }
    }
  }
}
```
<details><summary>Answer:</summary>
O(ab) where a = arrA.length and b = arrB.length  <br>
the if statement is O(1) work. <br>
</details>

---
```js
function foo (arrA, arrB) {
  for (i = 0; i < arrA.length; i++) {
    for (let j = 0; j < arrB.length; j++) {
      for (let k = 0; k < 100000; k++) {
        //
      }
    }
  }
}
```
<details><summary>Answer:</summary>
O(ab) - same as previous one <br>
because 100,000 units of work is constant work, it remains the same for all arrays hence it does not matter.
</details>

---
```js
function reverseArray (arr) {
  for (let i = 0; i < arr.length < 2; i++) {
    let other = array.length - 1 - i;
    let temp = array[i];
    array[i] = array[other];
    array[other] = temp;
  }
}
```
<details><summary>Answer:</summary>
O(N) - even though it goes thru only half of the list, we ignore the constants (N/ 2) = N;
</details>

---

##### Which of the following are equivalent to O(N)

```js
O(N + P) // where P < N / 2
O(2N)
O(N + log N)
O(N + M)
```
<details><summary>Answer:</summary>
1. N is dominant, hence equivalent. <br>
2. ignore constants. <br>
3. N >>> log N, hence equivalent <br>
4. we cant say anything about N and M, hence we need to keep both variables.
</details>

---

##### Suppose we have an array of strings, the algo takes the array, sorts each string, then sorts the full array. what is runtime?
<details><summary>Answer:</summary>
let S = length of longest string <br>
let A = length of array <br>
sorting each string is O(S log S) <br>
we have to do above for every string (which are A), hence O(A x S log S) <br>
we need to sort A string, which would take O(A log A) time, however, we must also take into account that we must compare the strings, which would take O(S) time, hence, the total time to sort the array would be O(S x A log A)<br>
Total time will be sum of the two terms = O(A x S(log A + log S))
</details>

---

##### this code sums the value of all nodes in a balanced binary tree, runtime?
```js
sum(node) {
  if (node == null) {
    return 0;
  }
  return sum(node.left) + node.value + sum(node.right);
}
```
<details><summary>Answer:</summary>
O(N) - two approaches to think about that <br>
- What the code means -> the code touches each node once, doing constant work, adding, (not counting the recursive calls), therefore, runtime will be linear in terms of number of nodes. hence O(N). <br>
- for recursive functions, runtime is O(branches^depth), here we have O(2^depth). <br>
  however, for n nodes, the depth is log n <br>
  hence, we are looking at O(2^logN) <br>
  if we take log as log base 2, we can reduce 2^logN <br>
  let P = 2^logN <br>
  take log on both sides -> log P = log 2^logN -> log P = logN log2 <br>
  -> P = N, hence 2^logN = N) <br>
  therefore, O(N) is the runtime.

---

##### The following method checks if a number is prime by checking for divisibility on numbers less than it. It only needs to go up to the square root of n because if n is divisible by a number greater than its square root then it's divisible by something smaller than it.
  
find time complexity

```js
isPrime(n) {
  for (let x = 2; x * x <= n; x++) {
    if (n % x === 0) {
      return false;
    }
  }
  return true
}
```
<details><summary>Answer:</summary>
O(sqrt of n) <br>
work inside the for loop is constant, In the worst case, the for loop runs for sqrt of n times, hence O(sqrt N).
</details>

---

```js
factorial (n) {
  if (n <= 1) {
    return 1
  } else {
    return n * factorial(n - 1);
  }
}
```

<details><summary>Answer:</summary>
O(n) - this is just simple recursion from n to 1.
</details>

---

##### The code below counts all permutations of a string.

```js
function permutations (string) {
  return permutation(string, "");
}

function permutation(str, prefix) {
  if (str.length === 0) {
    return prefix;
  } else {
    for (let i = 0; i < str.length; i++) {
      let rem = str.substring(0, i) + str.substring(i + 1);
      return permutation(rem, prefix + str[i]);
    }
  }
}
```
<details><summary>Answer:</summary>
O(n^2 x n!)
If we were to generate a permutation, then we would need to pick characters for each "slot:' Suppose we had 7 characters in the string. In the first slot, we have 7 choices. Once we pick the letter there, we have 6
choices for the next slot. (Note that this is 6 choices for each of the 7 choices earlier.) Then 5 choices for the
next slot, and so on.<br>
Therefore, the total number of options is 7 * 6 * 5 * 4 * 3 * 2 * 1, which is also expressed as 7! (7 factorial).
This tells us that there are n! permutations. Therefore, permutation is called n ! times in its base case (when prefix is the full permutation).<br>
_How many times does permutation get called before its base case?_<br>
But, of course, we also need to consider how many times lines 206 through 208 are hit. Picture a large call tree representing all the calls. There are n! leaves, as shown above. Each leaf is attached to a path of length n.
Therefore, we know there will be no more than n * n ! nodes (function calls) in this tree.<br>
_How long does each function call take?_
Executing line 204 takes O(n) time since each character needs to be printed.
Line 207 and line 208 will also take O (n) time combined, due to the string concatenation. Observe that the sum of the lengths of rem, prefix,and str[i] will always be n.
Each node in our call tree therefore corresponds to 0( n) work.<br>
What is the total runtime?<br>
Since we are calling permutation 0( n * n ! ) times (as an upper bound), and each one takes 0( n) time,
the total runtime will not exceed O ( n<sup>2</sup> x n ! ) .
</details>

---
##### The code below finds the nth fibonacci number

```js
function fib (n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;
  return fib(n - 1) + fib(n - 2);
}
```

<details><summary>Answer:</summary>
O(2<sup>n</sup>) -> following the O(branches ^ depth) rule for recursive functions.<br>
However, the actual runtime will be more like O(1.6^n), since sometimes there are only 1 recursive call instead of two, and this happens more as we reach the depth where there are more calls than the top, hence recursive calls start a decreasing trend as we get into more depth. 
</details>

---

##### Prints all Fibonacci numbers from 0 to n

```js
function allFib(n) {
  for (let i = 0; i < n; i++) {
    console.log(i + ": " + fib(i))
  }
}

function fib(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;
  return fib(n - 1) + fib(n - 2);
}
```
<details><summary>Answer:</summary>
O(2<sup>n</sup>) <br>
The wrong answer would be O(n x 2^n), even though the loop runs n times and each iteration of loop will take 2^n, however, n is changing inside the loop. it starts at 0, then goes to 1, 2, 3... so on. os total run times are 2^1 + 2^2 + 2^3 ... 2^n = 2^(n + 1), which is equivalent to O(2^n).
<br>
With memoization, the above function's runtime will be reduced to O(n).
</details>

example 16
