---
topic: 1D DP
difficulty: Easy
leetcode: https://leetcode.com/problems/min-cost-climbing-stairs/
---
# Min Cost Climbing Stairs

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/min-cost-climbing-stairs/)

> Each step = its cost + min of the previous two. Answer = min of the last two.

## Problem

You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

Follow up: How would your approach change if you were allowed to jump up to k steps instead of just one or two?: helper problem [Hard]: https://leetcode.com/problems/sliding-window-maximum/description/

## Notes

Similar to above.

Idea is same. the only thing is this time we are looking for min: ans[n] = min(ans[n-1],ans[n-2])

## Complexity

O(n)

## Code

```cpp
int size = static_cast<int>(cost.size());
        int step_1 = cost[1];
        int step_2 = cost[0];
        
        for(int i = 2; i < size; i++) {
            int next_step = cost[i] + min(step_1, step_2);
            step_2 = step_1;
            step_1 = next_step;
        }

        return min(step_1, step_2);
```
