# [Big O Cheatsheet](https://www.bigocheatsheet.com/)

# Bubble Sort

- it has a time complexity of $O(n^2)$.
- If the array is almost sorted, this improves to $O(n)$
- if done in place, has space complexity of $O(1)$

## Algo

- Start looping from the end to start (i)
- STart an inner loop from start to i,
- if arr[i] > arr[j], swap the elements.

#### Minor optimization

- we can keep a flag, that checks if there was a swap in the last iteration, if the number of swaps were zero then the array has been sorted.

# Selection Sort

- Time complexity of $O(n^2)$

## Algo

- STore the first element as the smallest value you've seen so far
- compare this item to the next item in the array unitl you find a smaller one
- if a smaller number is found, designate that smaller number to be the new minimum and conitnue until the end of the array.
- if the minimim is not the value(index) of current iterationm swap the two values
- repeat

# Insertion Sort

- Time complexity of $O(n^2)$
- If the array is almost sorted, this improves to $O(n)$
- Insertion sort is good for "online" data, we can place the data as it arrives in streams

## Algo

- start by picking the second element in the array
- Now compare the second element with the one before it and swap if necessary
- continue to the ntext element and if it is in the incorrect order, iterate through the sorted portion (i.e the left most portion) to place the element in the correct place
- Repeat.

# Merge Sort

- this has a time complexity of $O(n \log n)$ (Best, worst, average)
- Space complexity of $O(n)$
- the decomposition in recursion is $O(n)$, while the merge algo is $O(n)$.

## Algo

- Divide the array into two equal half.
- call merge sort on both of them.
- Repeat, till we reach an array of length 1, which is already sorted.
- Go back up, merging the sorted arrays as we go.
  - Merging algo: keep selecting the smallest element from both arrays and moving the two corresponsing pointers forward.
  - If one of the arrays is exhausted, all the elements of the other array are pushed into the result.
- the final array is merge sorted

# Quick Sort

- Relies, like merge sort, on the fact that list of 1 element is already sorted.
- time complexity $O(n \log n)$, worst time complexity of $O(n^2)$ and arises when the array is sorted or almost sorted.
- Space complexity is $O(n)$

## Algo

- Pick a pivot
- Divide the array into two groups, one larger and one smaller than the pivot.
- quicksort the smaller group, join it with the pivot, join is with the quicksort of larger group.

If we want constant space complexity, then the algorithm is little more complicated.

- Helper

  - The helper accepts 3 arguments, the `arr`, `start` index, `end` index (defaulted to 0 and arr.length - 1 respectively)
  - Grab the pivot (start, end, middle of array)
  - initiate a swapIndex from `start`
  - Lopp through the array from `start` until `end`
    - If the pivot is greater than the current element, increment the swapIndex
    - and then swap the current element with the elmenet at the swap index (after the increment to swapIndex)
  - After the loop, swap the pivot with the swapIndex (this places the pivot at the place whereever the swapIndex ended up being)
  - return the pivotIndex

- Main
  - call the `Helper` on initial `arr`
    - Base condition: `if (left < right) Do NOTHING;`
  - call `quickSort` on left side of the arr
  - call `quickSort` on the right side of the arr
  - return the sorted array

```js
function quickSort(arr, left = 0, right = arr.length - 1) {
	if (left < right) {
		const pivotIndex = helper(arr, left, right);
		// left side
		quickSort(arr, left, pivotIndex - 1);
		// right side
		quickSort(arr, pivotIndex + 1, right);
	}
	return arr;
}

function helper(arr, start = 0, end = arr.length - 1) {
	let pivot = arr[start];
	let pivotIndex = start;
	for (let i = start + 1; i <= end; i++) {
		if (pivot > arr[i]) {
			pivotIndex++;
			// swap the element at the swapIndex and the current element
			[arr[i], arr[pivotIndex]] = [arr[pivotIndex], arr[i]];
		}
	}

	// swap the pivot with the final value of swapIndex
	[arr[start], arr[pivotIndex]] = [arr[pivotIndex], arr[start]];
	return pivotIndex;
}
```

## Radix Sort

- All the algorithms above are **comparison sort** algorithms, meaning they need to compare elements to get the order.
- Mathematically, comparison sort at best give us $O(n \log n)$ time complexity.
- Radix sort, is not a comparison sort algo, rather it uses properties of numbers to sort the array.
- Radix sort only works for array of integers.
- Time complexity for Radix sort is $O(nk)$, where n is the lenght of the input array and k is the word length of the largest number in the array.

### Algo

#### Helpers

- `getDigit` at place: divide num by 10^place, drop the decimals, and mod by 10.
- `digitCount`: Log10 of num gives us word count - 1, we add 1 to the result. - for example digitCount(7654) => 10^x = 7654 => 10^3 + some decimal

#### Main

- figure out how many digits the largest number has, call it k
- loop from 0 to k - 1
- for each iteration
  - create buckets for each digit (0, 9)
  - place each number in the corresponding bucket based on its kth number
- replace our existing array with values in our buckets, starting with 0 and going up to 9.
- return the list.
