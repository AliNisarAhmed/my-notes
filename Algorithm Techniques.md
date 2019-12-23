# Algorithm Techniques

#### 1. Frequency Counters

In an object, count the frequencies of the letters in a string, and use it to count

e.g. for `validAnagrams`, (given two strings tell if they are anagrams)
```js
function validAnagram (str1, str2) {
  let obj = {};
  if (str1.length !== str2.length) return false;
  for (let char of str1) {
    obj[char] = obj[char] ? obj[char] + 1 : 1;
  }
  for (let char of str2) {
    if (!obj[char]) {  
    // either the string is not present, 
    // or its count has been reduced to ZERO
      return false;
    } else {
      obj[char]--;
    }
  }
  return true;
}
```

#### 2. Two pointers

Useful technique for counting values in an array/string

```js
function countUniqueValues (arr) {
  if (arr.length == 0) return 0;
  let i = 0; 
  for (let j = i + 1; j < arr.length; j++) {
    // j keeps moving forward until a unique value is found
    // then i is incremented, and arr[i] becomes that unique value
    if (arr[i] !== arr[j]) {  // a unique value
      i++;
      arr[i] = arr[j];
    }
  }
  return i + 1;
}
```

#### 3. Sliding Window 

Useful for keeping track of a subset of data in an array/string
```js
function maxSubArraySum(arr, n) {
  if (arr.length < n) return null;
  let maxSum = 0;
  let tempSum = 0;
  for (let i = 0; i < n; i++) {
    maxSum += arr[i];
  }
  // now that we have the sum of first n elements, we move the window
  // once to right,
  // and calculate the sum of the next "window", 
  // if that sum is greter than maxSum, we replace maxSum
  tempSum = maxSum;
  for (let i = n; i < arr.length; i++) {
    // sutracting 1 element to the left of window,
    // adding one element to the right
    tempSum = tempSum - arr[i - n] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```

#### 4. Divide and Conquer

Prime example is the **Binary Search**

#### 5. Helper Method Recursion

The function itself is not recursive, but inside it contains a recursive function.

This technique is used when we need to accumulate values in an array or object.
