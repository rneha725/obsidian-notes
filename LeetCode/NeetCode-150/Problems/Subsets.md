---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/subsets/
---
# Subsets

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/subsets/)

> Include or exclude each element. Or use a bitmask over 2^n.

## Problem

Get power set. Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

## Notes

Using backtracking, length of the inout is at max 2. So complaxity is also okay.

Backtrack:

void backtrack(const vector<int>& nums, int idx, vector<vector<int>> &res, vector<int> &curr_selection) {
 if(idx >= nums.size()) {
 res.push_back(curr_selection);
 return;
 }
 backtrack(nums, idx + 1, res, curr_selection);
 curr_selection.push_back(nums[idx]);
 backtrack(nums, idx + 1, res, curr_selection);
 curr_selection.pop_back();
 }

## Complexity

2^n

## Code

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
        int power_set_size = 1 << nums.size();
        vector<vector<int>> res(power_set_size);
        for(int i = 0; i < power_set_size; ++i) {
            int bit_num = i;
            int unit = 0;
            res[i].reserve(nums.size());
            while(bit_num) {
                if(bit_num & 1) res[i].push_back(nums[unit]);
                bit_num = bit_num >> 1;
                unit++;
            }
        }
        return res;
    }
```
