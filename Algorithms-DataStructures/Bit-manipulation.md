# Bit manipulation

## Xor 

- XOR of 0 and some bit is the same bit 
    - $a \oplus 0 = 0$ 

- XOR of two same bits is 0
    - $a \oplus a = 0$ 

- XOR is commutative and associative 
    - $a \oplus b \oplus a = (a \oplus a) \oplus b = 0 \oplus b = b$ 
    
We can use the above facts to find a single non-repeating number in an array

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        a = 0
        for i in nums:
            a ^= i   (^= is XOR in Python)
        return a
```

### Links 

https://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently


http://graphics.stanford.edu/~seander/bithacks.html


### Tricks

- To check if `ith` bit (from the right) of a binary number is set to 1.
     - `x & (1 << i) > 0` (if 0, the bit is not set. If 1, the bit is set to 1).
         - (a) - `1 << 2` take `1` and moves it two places left: `0100`
         - (b) - `5 & 4` = `0101 & 0100` = `0100` 
         - Since all numbers except the target bit in (a) are 0, `&` with the original number will be zero if it is zero in the original number, else it will be a number > 0.

- Set the `ith` bit (from the right) of a binary number
    - `x = x | (1 << i)`
        - (a) - `1 << i` will place a 1 at the `i` position. 
        - (b) - ORing with original number will get all the bits in the original number, plus it takes the `1` from (a) and puts it in original number.


- Clear the `ith` bit 
    - `x = x & ~(1 << i)`


- Extract last bit (not sure about these)
    - `x & ~x` OR 
    - `x & ~(x - 1)` OR 
    - `x ^ (x & (x - 1))`
    
- Remove the last 1 bit and replace it with 0
    - `x & (x - 1)`

- Set Union 
    - `a | b`
- Set Intersection 
    - `a & b`
- Set Subtraction 
    - `a & ~b`
