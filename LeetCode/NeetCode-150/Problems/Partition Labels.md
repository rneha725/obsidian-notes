---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/partition-labels/
---
# Partition Labels

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/partition-labels/)

> Store each char's last index. Extend the part to the furthest last index seen.

## Problem

You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string "ababcc" can be partitioned into ["abab", "cc"], but partitions such as ["aba", "bcc"] or ["ab", "ab", "cc"] are invalid.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.

Return a list of integers representing the size of these parts.

Example 1:

Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

## Notes

Straightforward - use vector instead of a map

## Complexity

O(n), S(1)

## Code

```cpp
vector<int> partitionLabels(string s) {
        vector<int> last_idx(26, 0);

        for(int i = 0; i < s.size(); i++) {
            last_idx[s[i] - 'a'] = i;
        }

        int idx = 0;
        vector<int> res;
        while(idx < s.size()) {
            int mx_idx = idx;
            for(int i = idx; i <= mx_idx; ++i) {
                mx_idx = max(mx_idx, last_idx[s[i] - 'a']);
            }
            res.push_back(mx_idx - idx + 1);
            idx = mx_idx + 1;
        }

        return res;
    }
```
