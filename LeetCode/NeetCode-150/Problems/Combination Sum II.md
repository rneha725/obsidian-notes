---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/combination-sum-ii/
---
# Combination Sum II

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/combination-sum-ii/)

> Sort. Skip duplicates at the same level. Each number used once.

## Problem

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

Example 1:

Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

## Notes

Subset | problem helped me with this, as I know why I needed to remove the duplicates and how.

Apart from that nothing much apart from the above soutions.

## Complexity

n*2^n, n

## Code

```cpp
//sorted nums
    void backtrack(int pos, const vector<int>& nums, int target, vector<vector<int>> &res, vector<int> &curr_sel) {
        if(target == 0) {
            res.push_back(curr_sel);
            return;
        }

        if(pos == nums.size() | target < 0) {
            return;    
        }

        for(int i = pos; i < nums.size(); ++i) {
            if(nums[i] > target) return; 
            if(i != pos && nums[i] == nums[i - 1]) continue;
        
            curr_sel.push_back(nums[i]);
            backtrack(i + 1, nums, target - nums[i], res, curr_sel);
            curr_sel.pop_back();
        }
    }
```
