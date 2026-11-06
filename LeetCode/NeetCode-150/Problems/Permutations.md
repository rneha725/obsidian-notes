---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/permutations/
---
# Permutations

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/permutations/)

> Swap each element into the current index, then recurse.

## Notes

Logic is to fix one number and swap with the curretn index and permute for remaining elements

## Complexity

n! time; n Space

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
