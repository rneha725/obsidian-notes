---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/edit-distance/
---
# Edit Distance

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/edit-distance/)

> min(insert, delete, replace) from the three neighbors. One row of space.

## Problem

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character

## Notes

Two-sequence string DP

Again having a formula thinking about the top down worked, but yes, this should be quicker now.
2d dp instantly provided space opmization in this.

Top down formula:

dp[i][j] = min(dp[i-1][j] + 1, dp[i][j-1] + 1, dp[i-1][j-1] + si==ri?0:1)

each entry in the min formula represent, insertion, deletion or replacement.

This question was a bit intimidating at first.

## Complexity

n*m, m

## Code

```cpp
int minDistance(string word1, string word2) {
        int n1 = static_cast<int>(word1.size());
        int n2 = static_cast<int>(word2.size());

        vector<int> dp(n2 + 1, 501);

        //first row seed
        for(int i = 0; i <= n2; ++i) {
            dp[i] = i;
        }
        
        for(int i = 1; i <= n1; ++i) {
            vector<int> curr(n2 + 1, 501);
            curr[0] = i;
            for(int j = 1; j <= n2; ++j) {
                int cost_replacing = 0;
                if(word1[i - 1] == word2[j - 1]) cost_replacing = dp[j - 1];
                else cost_replacing = dp[j - 1] + 1;

                int cost_del = 1 + dp[j];
                int cost_ins = 1 + curr[j - 1];

                curr[j] = min(cost_replacing, min(cost_del, cost_ins));
            }
            dp = curr;
        }

        return dp[n2];
    }
```
