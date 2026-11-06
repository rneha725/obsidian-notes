# Sliding Window: Progressive Questions

**Asked at (2025–26 reports):** Salesforce, Databricks, Booking, DocuSign, LinkedIn

**Signal:** longest / shortest / count of contiguous subarrays or substrings with a rule.

**Core idea:** Grow the right side. Shrink the left side while the window breaks the rule. For 'exactly K', use atMost(K) − atMost(K−1).

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 3 Longest Substring Without Repeating, 424 Longest Repeating Character Replacement, 567 Permutation in String, 76 Minimum Window Substring, 239 Sliding Window Maximum

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [904 Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/) | M | at most 2 distinct: the k-distinct template |  |  |
| [1004 Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/) | M | window with at most k zeros |  |  |
| [1658 Minimum Operations to Reduce X to Zero](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/) | M | flip it: longest window with sum total − x |  |  |
| [1838 Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/) | M | sort, then window on cost |  |  |
| [2958 Length of Longest Subarray With at Most K Frequency](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/) | M | count map in the window |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1438 Longest Continuous Subarray With Absolute Diff ≤ Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/) | M | two deques: window min and max. Salesforce asked window-min questions. |  |  |
| [992 Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/) | H | exactly K = atMost(K) − atMost(K−1). Salesforce asked a k-distinct variant. |  |  |
| [2444 Count Subarrays With Fixed Bounds](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/) | H | track last bad, last min, last max |  |  |
| [1499 Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/) | H | rewrite the formula, then a monotonic deque |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [862 Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/) | H | negatives allowed: deque over prefix sums |  |  |
| [480 Sliding Window Median](https://leetcode.com/problems/sliding-window-median/) | H | two heaps with lazy delete |  |  |
| [995 Minimum Number of K Consecutive Bit Flips](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/) | H | track active flips with a queue |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
