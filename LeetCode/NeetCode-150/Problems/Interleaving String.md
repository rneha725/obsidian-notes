---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/interleaving-string/
---
# Interleaving String

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/interleaving-string/)

> dp[i][j] = (s1 char matches and dp[i−1][j]) or (s2 char matches and dp[i][j−1]).

## Problem

Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.

An interleaving of two strings s and t is a configuration where s and t are divided into n and m substrings respectively, such that:

s = s1 + s2 + ... + sn
t = t1 + t2 + ... + tm
|n - m| <= 1
The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
Note: a + b is the concatenation of strings a and b.

Example: 

s1 = xyz
s2 = aabc

s3 = aaxbyzc
Output: true

No need to be bothered about this as this will always be true: |n - m| <= 1

## Notes

0/1 knapsack

It is hard to solve this one.

A good advice is to define your dp[i][j].

First I started with a recirsive solution. Then I could see the correlation among the various dp states, but still undertanding how was really hard, on top of that I needed to maintain the quality of the code which should looks easy.

In this problem, for base cases we needed sentinent list. 

Also, note that in case you are using a lot of i >= 0 etc for array boundaries, try to offset your main loop, if it makes sense, it helps simplify the code a lot.
Trick is nothing, the main formula is:

dp[i][j] = (s1[i] == s3[kl] AND dp[i - 1][j]) OR (s2[j] == s3[kl] AND dp[i][j - 1])

Code can still be simplified a lot. k can be derived from i and j.

## Complexity

T(n1*n2), S(n2)

## Code

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int n1 = static_cast<int>(s1.size());
        int n2 = static_cast<int>(s2.size());
        int n3 = static_cast<int>(s3.size());

        if(n3 != n1 + n2) return false;

        vector<bool> dp(n2 + 1, false);

        dp[0] = true;
        for(int j = 1; j < n2 + 1; ++j) {
            if(s3[j - 1] == s2[j - 1] && dp[j-1]) {
                dp[j] = true;
            }
        }

        for(int i = 1; i <= n1; ++i) {
            dp[0] = dp[0] && s1[i - 1] == s3[i - 1];
            for(int j = 1; j <= n2; ++j) {
                int k = i + j - 1;
                dp[j] = (s1[i - 1] == s3[k] && dp[j])| (s2[j - 1] == s3[k] && dp[j - 1]);
            }
        }

        return dp[n2];
    }
};
```
