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
