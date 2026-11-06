---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/jump-game-ii/
---
# Jump Game II

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/jump-game-ii/)

> Count a jump each time i passes the current window's end.

## Problem

Jump Game but with the minimum steps to reach destination.

## Notes

neat follow up in Jump Game.

Similar logic, this time you need to track a window for the current_reachable indices using the first number in the window and when we go out of the window, we can increase the num_of_steps.

## Complexity

O(n),. S(1)

## Code

```cpp
int size = static_cast<int>(nums.size());
        int res = 0;

        int curr_end = 0;
        int last_reachable_index = 0;

        for(int i = 0; i < size; ++i) {
            if(i > curr_end) {
                res++;
                curr_end = last_reachable_index;
            }
            last_reachable_index = max(last_reachable_index, i + nums[i]);
        }

        return res;
```
