---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/jump-game/
---
# Jump Game

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/jump-game/)

> Track the farthest reachable index. Fail if i ever passes it.

## Problem

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

## Notes

This one looks like dp, and it is quite straightforward to solve with dp, but dp gives O(n^2) complaxity. But greedy can do it in the O(n).

Trick is to start from the end and choose the indices as we move backwards, we need to pick the next reachable indices greedily, so if we can reach the current last_reachable index from the current, this one becomes the new last_reachable. 

In reteospect, knowing when to use dp and greedy looks tricky, Claude suggests to always go for greedy first and then check if making a local optimistic choice can gives us bad results in the future, if yes, then dp. Otherwise greedy. I will try to do this exercise in the upcoming questions.

One is in the another column, but this one the best and also the forward approach:

bool canJump(vector<int>& nums) {
 int size = static_cast<int>(nums.size());

 int last_jump_index = 0;

 for(int i = 0; i < size; ++i) {
 if(i > last_jump_index) return false;
 last_jump_index = max(last_jump_index, i + nums[i]);
 }

 return true;
 }

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
