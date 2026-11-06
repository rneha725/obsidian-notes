---
topic: 2d DP
difficulty: Hard
leetcode: https://leetcode.com/problems/burst-balloons/
---
# Burst Balloons

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/burst-balloons/)

> Interval DP. Pick the balloon burst LAST in [l, r]. Pad with 1s at both ends.

## Problem

You are given n balloons, indexed from 0 to n - 1. Each balloon is painted with a number on it represented by an array nums. You are asked to burst all the balloons.

If you burst the ith balloon, you will get nums[i - 1] * nums[i] * nums[i + 1] coins. If i - 1 or i + 1 goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it.

Return the maximum coins you can collect by bursting the balloons wisely.

## Notes

Quite hard to solve.

While coming onto the first pseudo solution is easy, but that does nto work

The trick here is to see the NeetCode Video.

It suggested to treat the current i as the index which will be burst at the end, this way you can rely on dp[left][i-1] and dp[i+1][right].

dp[i][j] gives us the max coins we can collect and at each iteration of dp, we fix the ith index, to be burst last so the cost will look like this: nums[left - 1]*nums[i]*nums[right + 1] + dp(left, i - 1, nums, memo) 
 + dp(i + 1, right, nums, memo);

I could not figure out a bottom up solution.

Added sentinent nodes.

## Complexity

o(n^3), -> number_of_states*time_spent_on_each_state

## Code

```cpp
int dp(int left, int right, const vector<int>& nums, vector<vector<int>> &memo) {
        if(left > right | right == 0 | left == 0 | right == nums.size() - 1 | left == nums.size() - 1) return 0; //check this

        if(memo[left][right] != -1) return memo[left][right];

        int res = 0;
        for(int i = left; i <= right; ++i) {
            int total_cost = nums[left - 1]*nums[i]*nums[right + 1] + dp(left, i - 1, nums, memo) 
                + dp(i + 1, right, nums, memo);

            res = max(res, total_cost);
        }

        return memo[left][right] = res;
    }
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(), 1);
        nums.insert(nums.end(), 1);
        vector<vector<int>> memo(nums.size(), vector<int>(nums.size(), -1));

        return dp(1, nums.size() - 2, nums, memo);
    }
```
