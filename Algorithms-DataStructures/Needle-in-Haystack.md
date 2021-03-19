# Needle in Haystack Algorithms

$N$ = length og Haystack
$L$ = Length of needle

## 1. Naive Approach

- Compare each substring with needle and see if any matches 
   
- Time Complexity = $O((N - L) \times L)$ 
- Space Complexicty = $O(1)$ 


## 2. Two Pointers

- Do not compare every substring
- Compare only those substring that start with the same character as the needle
- If the needle is not found, track back to the next character to the character where the search started

- Time Complexity = $O((N - L) \times L)$
- Space Complexity = $O(1)$ 

## 3. Rabin Karp 

Compare needle's hash with a rolling hash of haystack

Move along the haystack, generate hash of substring in the sliding window, and compare this rolling hash with the fixed hash of the needle.

##### Method: 

- Let's say needle and Haystack both consist of small English alphabet.
- Because strings are immutable in many languages, we must store the strings in arrays.
    - Hence, convert each array to their ASCII code and store it in array modulo $26$ (26 because we only deal with small-case characters)
- Calculating Rolling hashes
    - Consider:  `"abcd"` = `[0, 1, 2, 3]`
    - The hash is calculated as $h_0 = 0\times26^3+1\times26^2+2\times26^1+3\times26^0$ 
    - Now consider the next slice `"bcde"` = `[1, 2, 3, 4]`
    - New hash $h_1 = h_0 - (0\times26^3) \times 26 + 4 \times 26^0$   (multiply by 26 because the powers need to be shifted by one for the remainder of $h_0$)
    - In general, $h_1 = (h_0a - c_0a^L) + c_{L+1}$     where $a=26$ 
