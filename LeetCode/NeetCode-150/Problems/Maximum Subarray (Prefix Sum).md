---
topic: Prefix Sum
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-subarray/
---
# Maximum Subarray

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-subarray/)

> Best = current prefix − smallest prefix seen before it.

## Notes

Think about the the prefixSum and how can we maximise sum of a subarray: prefixSum[i] - prefixSum[j].

Track the current min of the prefi sum and keep on taking the max.

## Complexity

O(n)

## Code

```cpp
int prefixSum = 0;
        int currMin = 0;
        int res = INT_MIN;

        for (int i = 0; i < static_cast<int>(nums.size()); ++i) {
            currMin = min(currMin, prefixSum);
            prefixSum += nums[i];

            res = max(res, prefixSum - currMin);
        }

        return res;
```
