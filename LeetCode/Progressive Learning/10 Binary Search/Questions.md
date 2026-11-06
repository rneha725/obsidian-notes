# Binary Search: Progressive Questions

**Asked at (2025–26 reports):** Google, Apple, LinkedIn

**Signal:** sorted input, or 'minimize the maximum' / 'maximize the minimum'.

**Core idea:** Binary search on the answer: write can(x), then find the first x where it's true.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 704, 74, 875 Koko, 153, 33, 981 Time Based Key-Value Store, 4 Median of Two Sorted Arrays

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [162 Find Peak Element](https://leetcode.com/problems/find-peak-element/) | M | move toward the higher side |  |  |
| [540 Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/) | M | check pair parity |  |  |
| [81 Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) | M | duplicates break the rule |  |  |
| [1011 Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | M | binary search on the answer |  |  |
| [1482 Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) | M | same template |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [2064 Minimized Maximum of Products Distributed to Any Store](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store/) | M | same template, different check |  |  |
| [378 Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | M | search on the value, count ≤ mid |  |  |
| [410 Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | H | classic minimize-the-max |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [668 Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) | H | count ≤ mid per row |  |  |
| [719 Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) | H | sort + two-pointer count |  |  |
| [2141 Maximum Running Time of N Computers](https://leetcode.com/problems/maximum-running-time-of-n-computers/) | H | check uses min(battery, t) |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
