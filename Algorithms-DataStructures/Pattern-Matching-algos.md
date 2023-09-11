In the text below:
- `T` is the haystack
- `P` is the needle/pattern to find in T
- `n` = length of `T`
- `m` = length of `P`

# Brute Force

Efficiency = `O(mn)`


# Boyer-Moore Algorithm

The idea is to improve on Brute force using the following heuristics

1. *Looking-glass Heuristics*
	- When testing `P` in `T`, begin the comparisons from the end of `P` and move backward to the front of `P`
2. *Character-Jump Heuristic*
	- During search, a mismatch of text character `T[i] = c` with the corresponding pattern character `P[k]` is handled as follows:
		- if `c` is not contained anywhere in `P`, then shift `P` completely past `T[i]`, for it cannot match any character in `P`
		- else: shift `P` until an occurrence of character `c` in `P` gets aligned with `T[i]`

![[Pasted image 20230821191734.png]]

![[Pasted image 20230821192254.png]]

## Efficiency

The efficiency of Boyer-Moore algo relies on creating a lookup table that quickly determines where a mismatched character occurs elsewhere in the pattern.

We could use an array of the size of alphabet as a lookup table to store indices of matches.

However, using a hash table is more efficient rather than storing the whole alphabet array and initializing it.

The performance of Boyer-Moore is `O(nm + |Alpha|)` where `|Alpha|` represents the alphabet size of the language (e.g. 26 for English)


# KMP

Boyer-Moore has a major inefficiency:
- If we find several matching characters but then detect a mismatch, we ignore all the information gained by the successful comparisons after restarting with the next incremental placement of the pattern

KMP avoids this and achieves a runtime of `O(n + m)`


> The main idea of KMP is to pre-compute self-overlaps between portions of the pattern so that when a mismatch occurs at one location, we immediately know the maximum amount to shift the pattern before continuing the search.

![[Pasted image 20230821210736.png]]

## The failure function

KMP uses a "failure function" that indicates the proper shift of `P` upon a failed comparison.

Specifically, the failure function `f(k)` is defined as the length of the longest prefix of P that is a suffix of `P[1:k + 1]` (note `P[0]` is not included here, since we will shift at least one unit)
- Intuitively, if we find a mismatch upon character `P[k + 1]`, the function `f(k)` tells us how many of the immediately preceding characters can be reused to restart the pattern

![[Pasted image 20230821211209.png]]

In the above figure, the first `a` = `1` because it is the length of the longest prefix that matches the suffix `ma`

Similarly, `a=3` because `ama` at the beginning matches `ama` at `k=7`

### Examples

s = "aabaaa" -> fail = [0, 1, 0, 1, 2, 2]
s = "cgtacgttcgtac" -> fail = [0, 0, 0, 0, 1, 2, 3, 0, 1, 2, 3, 4, 5]

