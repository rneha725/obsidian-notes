---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-subarray/
---
# Maximum Subarray

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-subarray/)

> Kadane: extend the current run, or restart at this element.

## Problem

Given an integer array nums, find the subarray with the largest sum, and return its sum.

## Notes

Try brute force first, which comes out to be o(n^2) with the help of prefix sum.

Now a good stretch is either nLogn or n, in general Logn means binary search which requires the search set to be sorted. Unfortunately, we do not have that type of luxury. So we skip it and focus on optimizing till O(n).

Taking inspiration from maximum product subarray, this solutoin as also does a similar thing of either going with the selceted sub-array or just restarting the subarray.

So, we are calculating the sum, and leaving behind the subarray which gives us lesser sum than the current element value.

cumulative_max_sum = max(num[i], num[i] + cumulative_max_sum)

## Complexity

O(n); S(1)

## Code

```cpp
int current_max_sum = nums[0];
        int res = current_max_sum;

        for(int i = 1; i < nums.size(); ++i) {
            current_max_sum = max(nums[i], nums[i] + current_max_sum);

            res = max(res, current_max_sum);
        }

        return res;
```
