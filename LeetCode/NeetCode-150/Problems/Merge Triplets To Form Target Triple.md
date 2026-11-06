---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/merge-triplets-to-form-target-triple
---
# Merge Triplets To Form Target Triple

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/merge-triplets-to-form-target-triple)

> Skip triplets that exceed the target anywhere. Max the rest and compare.

## Problem

A triplet is an array of three integers. You are given a 2D integer array triplets, where triplets[i] = [ai, bi, ci] describes the ith triplet. You are also given an integer array target = [x, y, z] that describes the triplet you want to obtain.

To obtain target, you may apply the following operation on triplets any number of times (possibly zero):

Choose two indices (0-indexed) i and j (i != j) and update triplets[j] to become [max(ai, aj), max(bi, bj), max(ci, cj)].
For example, if triplets[i] = [2, 5, 3] and triplets[j] = [1, 7, 5], triplets[j] will be updated to [max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5].
Return true if it is possible to obtain the target triplet [x, y, z] as an element of triplets, or false otherwise.

## Notes

Straightforward - simplifythe loops and if/else

## Complexity

O(n), S(1)

## Code

```cpp
int val1 = 0;
        int val2 = 0;
        int val3 = 0;

        for(const vector<int> &triplet: triplets) {
            bool is_valid_triplet = true;
            if(triplet[0] > target[0] | triplet[1] > target[1] | triplet[2] > target[2]) is_valid_triplet = false;

            if(is_valid_triplet) {
                val1 = max(val1, triplet[0]);
                val2 = max(val2, triplet[1]);
                val3 = max(val3, triplet[2]);

                if(val1 == target[0] && val2 == target[1] && val3 == target[2]) return true;
            }
        }

        return false;
```
