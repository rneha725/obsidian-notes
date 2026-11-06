---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-length-of-pair-chain/
---
# Maximum Length of Pair Chain

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-length-of-pair-chain/)

> Tried as DP, but it is really greedy. See the Greedy version.

## Problem

Return the length longest chain which can be formed.

## Notes

DP Pattern: Intervals, 1D

Actually greedy and not dp.
For greedy solution we sort using the end times and select the next interval ofthe chain accroding the last interval's end time. Last end time < next_end_time.
Same as above with no earnings array to optimize just the length.

## Complexity

nLogn
