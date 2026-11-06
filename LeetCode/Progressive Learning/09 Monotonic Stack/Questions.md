# Monotonic Stack: Progressive Questions

**Asked at (2025–26 reports):** DocuSign

**Signal:** next greater / smaller, spans, 'how far can it see', sum over subarray minimums.

**Core idea:** Keep the stack sorted. The moment you pop is the moment you learn an answer. Store indices, not values.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 155 Min Stack, 739 Daily Temperatures, 853 Car Fleet, 84 Largest Rectangle in Histogram, 42 Trapping Rain Water

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [503 Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/) | M | circular: loop 2n times |  |  |
| [901 Online Stock Span](https://leetcode.com/problems/online-stock-span/) | M | previous greater, streaming |  |  |
| [1019 Next Greater Node In Linked List](https://leetcode.com/problems/next-greater-node-in-linked-list/) | M | same idea, different input |  |  |
| [402 Remove K Digits](https://leetcode.com/problems/remove-k-digits/) | M | increasing stack builds the smallest number |  |  |
| [456 132 Pattern](https://leetcode.com/problems/132-pattern/) | M | scan right to left |  |  |

## Stage 2: Twists (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [316 Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/) | M | 402 + seen set + last index |  |  |
| [962 Maximum Width Ramp](https://leetcode.com/problems/maximum-width-ramp/) | M | decreasing stack, then scan from the right |  |  |
| [907 Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/) | M | contribution = left × right; handle duplicates |  |  |
| [2104 Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges/) | M | 907 for max and for min |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [85 Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/) | H | run 84 on every row |  |  |
| [1944 Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/) | H | count pops per person |  |  |
| [2289 Steps to Make Array Non-decreasing](https://leetcode.com/problems/steps-to-make-array-non-decreasing/) | M | stack + DP; harder than it looks |  |  |
| [2281 Sum of Total Strength of Wizards](https://leetcode.com/problems/sum-of-total-strength-of-wizards/) | H | 907 + prefix of prefix sums |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
