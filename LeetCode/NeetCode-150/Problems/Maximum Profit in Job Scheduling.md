---
topic: 1D DP
difficulty: Hard
leetcode: https://leetcode.com/problems/maximum-profit-in-job-scheduling/
---
# Maximum Profit in Job Scheduling

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

> Sort by start. dp[i] = max(take: profit + dp[next job via lower_bound], skip: dp[i+1]).

## Problem

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i].

You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

## Notes

DP Pattern: Intervals

This was asked in Uber screening in April 26. I gave a bad answer tbh. Notion

One solution I tried was, with bottom up DP where we are trying to memoize for an index, from the end, that what is the max earnings which can be achieved from i to the end of the array. The trick it to use the lower_bound. The other is to consider both selecting and not selecting the element. 

When we select an element we find the lower bound with the end time of the selected interval and as the mem[lower_bound_index] will have the solution starting from the selected index + currentProfit.
Trick is that when we not select an element, the other solution could be mem[j+1], if we are trying to find the solution for jth index.

## Complexity

nLog(n)

## Code

```text
sort jobs by start time

mem[i] = profit[i]  for all i

for j from n-2 down to 0:
    next = lower_bound(startTime, endTime[j])  // first job starting >= end of j
    
    mem[j] = max(
        profit[j] + mem[next],  // take j
        mem[j+1]                // skip j
    )
return mem[0]
```
