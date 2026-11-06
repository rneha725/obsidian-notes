---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-product-subarray/
---
# Maximum Product Subarray

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-product-subarray/)

> Track both max and min so far. A negative number swaps them.

## Notes

I tried a lot to solve it using miimal logic, by taking care of zeroes and seeing that we can have two negatives and we are all good. But even after realising this coming up with the solution was tricky. I think it belongs to the puzzle category, although it was tagged as dp.

The trick it to keep 3 variables in check
- current_number
- max_so_far
- min_so_far

and we will go about this as:
min_so_far = min(current_number, max_so_far_current_number, min_so_far_current_number)
max_so_far = max(current_number, max_so_far_current_number, min_so_far_current_number)

and updating the resultant with the new calculations of the max_so_far

## Complexity

Time: O(n)

## Code

```cpp
int max_so_far = 1;
        int min_so_far = 1;
        int res = INT_MIN;

        for(int i = 0; i < nums.size(); ++i) {
            int tmp_max = max_so_far;

            max_so_far = max(nums[i], max(nums[i]_max_so_far, min_so_far_nums[i]));
            min_so_far = min(nums[i], min(nums[i]_tmp_max, min_so_far_nums[i]));
            res = max(res, max_so_far);
        }

        return res;
```
