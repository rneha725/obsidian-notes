---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/palindromic-substrings/
---
# Palindromic Substrings

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/palindromic-substrings/)

> Not DP. Expand around every center and count.

## Problem

Count all the palindromes in a string.

## Notes

Not a dp.

Expand from center for the odd and even palindrome.

## Complexity

Time: O(n^2)
Space: O(1)

## Code

```cpp
int size = static_cast<int>(s.size());
        int res = 0;

        for(int i = 0; i < size; ++i) {
            auto odd_size = expand(i, i, s);
            res += i - odd_size.first + 1;
            
            auto even_size = expand(i, i + 1, s);
            res += i - even_size.first + 1;
        }
        return res;
```
