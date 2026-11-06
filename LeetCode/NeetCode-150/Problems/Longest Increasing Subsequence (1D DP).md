---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/longest-increasing-subsequence/
---
# Longest Increasing Subsequence

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/)

> Keep a tails array. Append if bigger, else binary search and replace. O(n log n).

## Problem

Given an integer array nums, return the length of the longest strictly increasing subsequence.

Follow up: return the sequences also.

## Notes

The question asks to return the length and nothing else.

In that case, we will push into a vector, when we encounter something bigger than the last number of the array, if we find something which is not greater than the last element, then we use binary search to find its position such that we either find number equal to the current number or greater.

## Complexity

O(n Logn)

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
