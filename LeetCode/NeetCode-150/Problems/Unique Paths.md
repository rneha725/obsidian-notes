---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/unique-paths/
---
# Unique Paths

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/unique-paths/)

> One-row DP, or the math answer C(m+n−2, m−1).

## Problem

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

## Notes

Initially I solved it using simple dp, mxn matrix and used simple non-recursive approach, It worked in time and space n^2. To save space just use one dimenstion storage.
Other save was to use math itself and handle the overflows. While I am satisfied with the BFS, math can solve this in O(n) time:

the logic is that every path length is equal and we need to make m + n - 2 decisions for creating each path, and we need to make exactly m - 1 down decisions to create a path or n - 1 right decisions to create a path. Answer: C(m + n - 2, m - 1) or n - 1

long double combination(int nem, int deno) {
 deno = min(nem, nem - deno);
 long double product_nem = 1;
 long double product_deno = 1;

 for(long double i = deno; i >= 1; --i) {
 product_nem *= (nem - i + 1);
 product_deno *= i;
 }

 return (long double)product_nem/(long double)product_deno;
 }
 int uniquePaths(int m, int n) {
 return ceil(combination(m + n - 2, m - 1)); 
 }

## Complexity

For the dp:

O(nxm), space: O(n)

## Code

```cpp
vector<int> ways(n, 1);

        for(int i = 1; i < m; ++i) {
            vector<int> ways_new(n, 0); 
            ways_new[0] = 1;
            for(int j = 1; j < n; ++j) {
                ways_new[j] = ways_new[j - 1] + ways[j];
            }
            ways = ways_new;
        }

        return ways[n - 1];
```
