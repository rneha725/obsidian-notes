---
topic: 1D DP
difficulty: Easy
leetcode: https://leetcode.com/problems/climbing-stairs/
---
# Climbing Stairs

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/climbing-stairs/)

> ways[n] = ways[n−1] + ways[n−2]. Only two variables needed.

## Problem

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Notes

Dp pattern: 1d with no need of array

we need to understand this: ans[n] = ans[n-1]+ans[n-2]

if you will try this on a seed, try n = 2.ans[2] = ans[1]+ans[0]. -> this is how we need to seed the values and move on from the first indices to last. We also do not need an array to store all this as we only need two values.

## Complexity

O(n)

## Code

```cpp
if(n == 1) return 1;

        int steps_2 = 1;
        int steps_1 = 1;

        for(int i = 2; i < n; i++) {
            int steps_curr = steps_2 + steps_1;
            steps_2 = steps_1;
            steps_1 = steps_curr;
        }

        return steps_2 + steps_1;
```
