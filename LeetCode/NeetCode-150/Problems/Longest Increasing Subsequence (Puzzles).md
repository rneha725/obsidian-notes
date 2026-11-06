---
topic: Puzzles
difficulty: Medium
leetcode: https://leetcode.com/problems/longest-increasing-subsequence/
---
# Longest Increasing Subsequence

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/)

> Patience sorting view of the O(n log n) LIS.

## Problem

Given an integer array nums, return the length of the longest strictly increasing subsequence.

Follow up: return the sequences also.

## Notes

_No notes yet._

## Complexity

O(n Logn)

It i a game of solitaire: https://leetcode.com/discuss/post/1129459/patience-sorting-dp-optimisation-by-praj-ecnm/

## Code

```cpp
vector<int> seq;
        seq.push_back(nums[0]);
        
        for(int i = 1; i < nums.size(); ++i) {
            if(seq[seq.size() - 1] < nums[i]) {
                seq.push_back(nums[i]);
            } else if(seq[seq.size() - 1] > nums[i]) {
                int lb_idx = binary_search(seq, nums[i]); //target >= array[mid]
                seq[lb_idx] = nums[i];
            }
        }

        return seq.size();
```
