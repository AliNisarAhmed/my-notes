# Common Algorithms

## Binary Search

- Time complexity of Binary search in `O(log n)`.
  - The log has a base of `2`. this is because we divide the array into half on each iteration.
- A simple binary search algo uses the two pointer technique, and manages those pointers based on if the midpoint elem of the current section matches the target.

<details>
  <summary>CSharp</summary>

````csharp
    public static int Search<T>(IEnumerable<T> arr, T target) where T: IComparable
    {
        var left = 0;
        var right = arr.Count() - 1;
        int mid;
        while (left <= right)
        {
            mid = (left + right) / 2;
            var current = arr.ElementAt(mid);
            if (current.Equals(target))
            {
                return mid;
            }
            else if (current.CompareTo(target) < 0)
            {
                // if target > current, that means we need to search the right section next
                // so we bring the "left" up
                left = mid + 1;
            }
            else
            {
              // if target < current, we need to bring the "right" down to search the left section
                right = mid - 1;
            }
        }
        return -1;
    }
    ```
</details>
````

---

## Naive String Search vs KMP algorithm

- Native string search loops through a long string to find instances of the short string.
- it is `O(m n)` algorithm.
- KMP Algo: https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/
- https://medium.com/@giri_sh/string-matching-kmp-algorithm-27c182efa387
- KMP improves on the algorithm by pre-processing the pattern to generate an array of integers. This array is called Ips.
