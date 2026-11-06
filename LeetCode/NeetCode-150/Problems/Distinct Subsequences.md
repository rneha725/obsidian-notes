---
topic: 2d DP
difficulty: Hard
leetcode: https://leetcode.com/problems/distinct-subsequences/
---
# Distinct Subsequences

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/distinct-subsequences/)

> If chars match: take diagonal + left, else just left.

## Problem

Given two strings s and t, return the number of distinct subsequences of s which equals t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

Example 1:

Input: s = "rabbbit", t = "rabbit"
Output: 3

## Notes

Easy dp with 2d.

## Complexity

n*m; n

## Code

```cpp
int numDistinct(string s, string t) {
        int n1 = static_cast<int>(s.size());
        int n2 = static_cast<int>(t.size());
        vector<unsigned long long> dp(n1, 1);

        for(int i = 0; i < n2; ++i) {
            vector<unsigned long long> curr(n1, 0);
            if(t[i] == s[i]) {
                if(i >= 1) curr[i] += dp[i - 1];
                else curr[i] = 1;
            }

            for(int j = i + 1; j < n1; ++j) {
                if(t[i] == s[j]) {
                    curr[j] = curr[j - 1] + dp[j - 1];
                } else {
                    curr[j] = curr[j - 1];
                }
            }
            dp = curr;
        }
        return static_cast<int>(dp[n1 - 1]);
    }
```
