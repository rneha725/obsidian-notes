---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/house-robber/
---
# House Robber

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/house-robber/)

> dp[i] = max(dp[i−1], nums[i] + dp[i−2]).

## Problem

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police. it will automatically contact the police if two adjacent houses were broken into on the same night.

## Notes

My bad:
My aprroach was to look at an example and for each i, get the max possible for that. so my formula was dp[n] = n_val + max(dp[n-2], dp[n-1]) as this was covering all the paths.

But the real solution was using the same approach as the top down. So what we are saying is, we eight say current one is the max or the last value has the max value. dp[n] = max(dp[n-1], nums[i] + dp[n-2])

## Complexity

O(n)

## Code

```cpp
int size = static_cast<int>(nums.size());
        int prev = 0;
        int curr = 0;
        for(int i = 0; i < size; ++i) {
            int next = max(curr, prev + nums[i]);
            prev = curr;
            curr = next;
        }

        return curr;
```
