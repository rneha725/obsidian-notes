---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/partition-equal-subset-sum/
---
# Partition Equal Subset Sum

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/partition-equal-subset-sum/)

> 0/1 knapsack to reach sum/2. Loop the target backwards.

## Problem

Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.

## Notes

Dp Pattern: 0-1 knapsack
Pattern is that we need to reach a target number by picking some/all elements from the given array.

For this type of knapsack, where we have an element to choose only once and we have a target we do this:
we visualize it as a 2d array. One side is the array's elements, other is the target starting from 1/0 whaetver is suitable. We calculate the initial values without iteration. Then we need to loop the target value from tthe end and choose the next element in the array and calcualte the new elements as: dp[curr] = calcualte(dp[curr], dp[curr-curr_target_val]).

for(int i = 1; i <= size; ++i) {
 int ele = nums[i - 1];
 for(int j = target; j >= ele; --j) {
 dp[j] = dp[j] | dp[j - ele];
 }
 }

## Complexity

O(n*target)

## Code

```cpp
int size = nums.size();

        int sum = 0;
        for(int n: nums) sum += n;

        if(sum % 2 == 1) return false;
        int target = sum/2;
        
        vector<bool> dp(target + 1, false);
        dp[0] = true;

        for(int i = 1; i <= size; ++i) {
            int ele = nums[i - 1];
            for(int j = target; j >= ele; --j) {
                dp[j] = dp[j] | dp[j - ele];
            }
        }

        return dp[target];
```
