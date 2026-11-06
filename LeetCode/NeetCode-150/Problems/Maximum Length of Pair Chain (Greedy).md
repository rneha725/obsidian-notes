---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-length-of-pair-chain/
---
# Maximum Length of Pair Chain

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-length-of-pair-chain/)

> Sort by end. Take a pair if its start > last end.

## Problem

Return the length longest chain which can be formed.

## Notes

This is a greedy problem, we can sort using the end time and see if the next has the start time > last_end_time.

My bad: I used dp to solve it.

## Complexity

O(n)

## Code

```cpp
sort(pairs.begin(), pairs.end(), [](https://claude.ai/chat/const%C2%A0vector%3Cint%3E%C2%A0&a,%C2%A0const%C2%A0vector%3Cint%3E%C2%A0&b) {
            return a[1] < b[1];
        });

        int last_end = INT_MIN;
        int count = 0;
        for(auto pair: pairs) {
            if(pair[0] > last_end) {
                count++;
                last_end = pair[1];
            }
        }
        return count;
```
