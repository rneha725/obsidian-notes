---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/house-robber-ii/
---
# House Robber II

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/house-robber-ii/)

> Run House Robber twice: once without the last house, once without the first. Take the max.

## Problem

Just like above but this time we need deal with a circular array

## Notes

My bad:
I tried it with recursion, that looked complicated, as I was using a flag and setting it during the recursion if the i ==0 is there in the recuirsion stack. Easier would be to have two calls of dp, one called from 0 and the other from 1 and then we can use the fucntion of dp, where it will take care of the flag and the last element. 

if(flag and i is last element) return 0

Also we needed to set the mem arry for two dp calls.

for simplicity use bottom up:

We need two executions like above, one which will start from zero and skip the last element.
Other, start from 1 and keep the last element.

Max from the above two.

## Complexity

O(n)

## Code

```cpp
int rob(vector<int>& nums) {
        int size = static_cast<int>(nums.size());
        if(size == 1) return nums[0];
        int max1 = calculate(nums, 0, size - 2);
        int max2 = calculate(nums, 1, size - 1);
        return max(max1, max2);
    }

    int calculate(vector<int>& nums, int start, int end) {
        int prev = 0;
        int curr = 0;
        for(int i = start; i <= end; ++i) {
            int next = max(curr, nums[i] + prev);
            prev = curr;
            curr = next;
        }
        return curr;
    }
```
