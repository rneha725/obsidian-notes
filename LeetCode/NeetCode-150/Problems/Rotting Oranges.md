---
topic: Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/rotting-oranges/
---
# Rotting Oranges

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/rotting-oranges/)

> Multi-source BFS from rotten oranges. Each BFS level is one minute.

## Problem

Follow up: 
1. How would your logic change if some oranges were 'shielded' and required two adjacent rotten neighbors to rot instead of just one?
2. How would you modify your approach if the grid was wrapped like a torus where oranges could rot across the boundaries?

## Notes

Same as above, seed with the rotten oranges, then bfs. It was simpler, no need of visited as such, because BFS will always fill a cell with the nearst possible distances for uniform weight cases.

Note use: static constexpr array<array<int, 2>, 4> directions = {{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}};

instead of vector<vector<int>> directions;

read like const auto &dir: directions -> much faster

## Complexity

Time: O(mxn) 
Space: O(mxn)
