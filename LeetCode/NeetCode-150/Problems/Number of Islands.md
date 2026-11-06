---
topic: Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/number-of-islands/
---
# Number of Islands

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/number-of-islands/)

> For each unvisited '1', BFS to mark the whole island. Count the BFS calls.

## Problem

Question: Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## Notes

This is about finding the 1s and then traversing. Traversal can be done using the BFS/DFS/UF. As talking to claude, using BFS could be the best strategy. As it is simple and the space complexity is min(m, n). 
DFS space will be mxn considering a snake like traversal, which can overflow the memory. Use a visited array with BFS so we are not mutating the original input.

Explanation: 
Why BFS gives O(min(m,n)):

BFS explores level by level. The queue only holds the current frontier — cells at the wavefront radiating outward from the source. In a grid, that wavefront can never be wider than the smaller dimension, so the queue is bounded by O(min(m,n)).

Why iterative DFS with explicit stack does NOT:

The stack follows a path, not a frontier. At each cell you push up to 4 neighbors before processing any of them. In the worst case (a winding snake-like island through all cells), the stack accumulates O(m*n) entries.

BFS over DFS:
The one-line heuristic:

If the problem asks for minimum/shortest → BFS.
If it asks for existence, all solutions, or structure → DFS.

## Complexity

Time: O(m×n) — every cell visited at most once
Space: O(min(m,n)) — max BFS queue width

## Code

```text
numIslands(grid):
    count = 0
    for each cell (r, c) in grid:
        if grid[r][c] == '1':
            BFS(grid, r, c)
            count++
    return count

BFS(grid, r, c):
    mark (r,c) as visited
    queue = [(r, c)]
    while queue not empty:
        (r, c) = queue.pop_front()
        for each neighbor (nr, nc) in 4 directions:
            if in bounds AND grid[nr][nc] == '1':
                mark (nr, nc) as visited
                queue.push(nr, nc)
```
