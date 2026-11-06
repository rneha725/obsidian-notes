---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/longest-palindromic-substring/
---
# Longest Palindromic Substring

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/longest-palindromic-substring/)

> Not DP. Expand around every center, odd and even.

## Problem

Longest palindrome in a string

## Notes

Not a dp.

Expand from center for the odd and even palindrome.

## Complexity

Time: O(n^2)
Space: O(1)

## Code

```cpp
int size = static_cast<int>(s.size());
        pair<int, int> res_idx = {0, 1};

        for(int i = 0; i < size; ++i) {
            auto odd_size = expand(i, i, s);
            if(odd_size.second - odd_size.first - 1 > res_idx.second - res_idx.first - 1) res_idx = odd_size;
            
            auto even_size = expand(i, i + 1, s);
            if(even_size.second - even_size.first - 1 > res_idx.second - res_idx.first - 1) res_idx = even_size;
        }
        cout<<res_idx.first<<endl;
        return s.substr(res_idx.first + 1, res_idx.second - res_idx.first - 1);
```
