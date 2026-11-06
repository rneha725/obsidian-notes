---
topic: Sliding Window
difficulty: Hard
leetcode: https://leetcode.com/problems/sliding-window-maximum/
---
# Sliding Window Maximum

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/sliding-window-maximum/)

> Monotonic deque of candidates. Front is the max; drop expired ones from the front.

## Notes

This is tricky and unintuitive.

First guess was to use a map, which will store the elements of the current window in a frequency map, this played out good but the complexoity was n*logk. The more optimal solution is O(n).

Mental model: a line of candidates

Think of the deque as a queue of candidates for max, ordered best → worst front to back.

New element arrives → knockout tournament from the back (beat smaller ones, take their spot)
Window slides → oldest candidate at the front retires if expired
Answer → always the front (best surviving candidate)

## Complexity

Time:O(n)
Space: O(k)
