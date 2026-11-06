---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/longest-common-subsequence/
---
# Longest Common Subsequence

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/longest-common-subsequence/)

> Match → diagonal + 1, else max(up, left). Two rows, 1-indexed.

## Problem

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

## Notes

I solved it initially using recursion, and for the index which are equal I stll considered 3 branches, which is to searcha again considering the match, and not considering the match. We always consider the match btw. I should have thought about it. I memoized using the 2d matrix, but in google they find it better to be solved using bottom up.

SO I followed the same game to convert it into the bottom up, one optimzation was to use only two arrays, prev and current.

One code simplefication: to 1 index these arrays which removed a lot of conditional checks and the solution was each then.

## Complexity

O(m*n), Space: O(m)

## Code

```cpp
vector<int> prev(text2.size() + 1, 0);
        vector<int> curr(text2.size() + 1, 0);

        for(int i = 1; i <= text1.size(); ++i) {
            fill(curr.begin(), curr.end(), 0);
            for(int j = 1; j <= text2.size(); ++j) {
                if(text1[i - 1] == text2[j - 1]) {
                    curr[j] = prev[j - 1] + 1;
                } else {
                    curr[j] = max(prev[j], curr[j - 1]);
                }
            }

            swap(prev, curr);
        }

        return prev[text2.size()];
```
