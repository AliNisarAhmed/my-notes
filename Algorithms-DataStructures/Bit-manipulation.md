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
