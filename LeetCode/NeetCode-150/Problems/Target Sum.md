---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/target-sum/
---
# Target Sum

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/target-sum/)

> DP over all reachable sums using an offset. next[j] = dp[j − num] + dp[j + num].

## Problem

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

## Notes

0/1 knapsack

I started with the recursion, and it was then easy to convert that into the bottom up.

Some problems were that I handled zeroes differently which should not be the case.

Bottom up was easy once you know how to calculate the values.

I was confused how to seed, then realised no need to seed if we just set dp[0] = 1 before starting the loop and we can start from 0th index in the num.

Converting from recursion to bottom up was one thing.
then sumplifying the bottom up approach was one.

Finding the right seed value in DP is needed to solve.

## Complexity

T(n*s); S(s)

## Code

```cpp
const int SUM = std::accumulate(nums.begin(), nums.end(), 0);
        const int OFFSET = SUM;

        if(target < -SUM | target > SUM) return 0;

        vector<int> dp(2_OFFSET + 1, 0);
        dp[OFFSET] = 1;

        for(int num: nums) {
            vector<int> next(2_OFFSET + 1, 0);
            for(int j = -SUM + OFFSET; j <= SUM + OFFSET; ++j) {
                if(j - num >= 0 && j - num < dp.size()) {
                    next[j] += dp[j - num];
                }
                if(j + num >= 0 && j + num < dp.size()) {
                    next[j] += dp[j + num];
                }
            }
            dp = next;
        }

        return dp[OFFSET + target];
```
