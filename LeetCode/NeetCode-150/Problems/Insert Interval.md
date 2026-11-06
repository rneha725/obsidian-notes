---
topic: Intervals
difficulty: Medium
leetcode: https://leetcode.com/problems/insert-interval/
---
# Insert Interval

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/insert-interval/)

> Three phases: add intervals before, merge the overlapping ones, add the rest.

## Notes

Claryfying your approach well before is important, this question is easy but also you have some options while solving it, so first choose one and go through it. Do not skip it as it affects the following steps and the confidence with your own approach. Make it a habit.

So inserting the intervals one by one, checking for each if the new interval should come before it, if yes, then merge with the last pushed interval, aafter that keep on merging.

Better to do in the 3 phases, 1 merge the intervals before the new interval, then merge with the new interval all the mergables, then merge the remaining.

## Complexity

O(n); S(n)

## Code

```cpp
// Phase 1: intervals that end before newInterval starts (no overlap).
        while (i < n && intervals[i][1] < newInterval[0]) {
            res.push_back(intervals[i++]);
        }

        // Phase 2: merge all overlapping intervals into newInterval.
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            ++i;
        }
        res.push_back(newInterval);

        // Phase 3: intervals that start after newInterval ends (no overlap).
        while (i < n) {
            res.push_back(intervals[i++]);
        }
```
