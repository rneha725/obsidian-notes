---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-number-of-points-with-cost/
---
# Maximum Number of Points with Cost

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

> Left-to-right and right-to-left running max give each cell's best in O(1).

## Problem

For every two adjacent rows r and r + 1 (where 0 <= r < m - 1), picking cells at coordinates (r, c1) and (r + 1, c2) will subtract abs(c1 - c2) from your score.

Return the maximum number of points you can achieve.

## Notes

This is mostly dp optimiaztion. Starting with dp optmization helped, and knowing what complexity do we need to achieve helped. 

Brute is m*(n^2) and we need to achieve m*n.

For that for each element in row we need to know in O(1) time what is the max we can achieve.

I have done this in two loops, one which keeps track of the left max and populates a temp vector, same for right max.

for(int i = 1; i < n_row; ++i) {
 vector<long long> curr_row(n_col, 0);
 
 long long left_max = 0;
 //left iteration
 for(int j = 0; j < n_col; ++j) {
 left_max = max(--left_max, prev_row[j]);
 curr_row[j] = points[i][j] + left_max;
 }

 long long right_max = 0;
 //right iteration
 for(int j = n_col - 1; j >= 0; --j) {
 right_max = max(--right_max, prev_row[j]);
 curr_row[j] = max(curr_row[j], points[i][j] + right_max);
 }

 prev_row = curr_row;
 }

## Complexity

O(m*n), S(n)

## Code

```cpp
for(int i = 1; i < n_row; ++i) {
            vector<long long> curr_row(n_col, 0);
            
            long long left_max = 0;
            //left iteration
            for(int j = 0; j < n_col; ++j) {
                left_max = max(--left_max, prev_row[j]);
                curr_row[j] = points[i][j] + left_max;
            }

            long long right_max = 0;
            //right iteration
            for(int j = n_col - 1; j >= 0; --j) {
                right_max = max(--right_max, prev_row[j]);
                curr_row[j] = max(curr_row[j], points[i][j] + right_max);
            }

            prev_row = curr_row;
        }
```
