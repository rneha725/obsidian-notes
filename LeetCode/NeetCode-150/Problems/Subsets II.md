---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/subsets-ii/
---
# Subsets II

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/subsets-ii/)

> Sort. Skip the same value at the same tree level. Retry this one.

## Problem

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

## Notes

I struggled to solve this as this has duplicates, I came up with a soltuon which used a frequency key:value types vector.

You select the element (pushing in the curr selection), then decrement the frequency.
this is done in a for loop on the freq vector, so we are always moving forward.

Although when I tried this on the original array by sorting it, it did not work, I removed the map completely and kept an index from which I should start my processing.

The thing is this is backtracking and every selection is pushed at the end of processing a node in the backtrack tree.

Retry this

in the backtracking tree, I can say we do not want to repeat the same number branch and we need to sort the input.
