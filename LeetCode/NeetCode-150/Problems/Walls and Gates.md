---
topic: Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/walls-and-gates/
---
# Walls and Gates

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/walls-and-gates/)

> Multi-source BFS from all gates. No Dijkstra: every step costs 1.

## Problem

You are given an m x n grid rooms initialized with these three possible values.

-1 A wall or an obstacle.
- 0 A gate.
- INF Infinity means an empty room. 
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

Follow up: if we need to get to the key to open a door instead.

## Notes

Mistakes:
- I initiallya used a dijsktra like solution, where all the gates were first pushed into a pq. I missed the point that distances are increasing by 1, so no need of Dijstra, instead this is multi source BFS problem.
- I gave the complexitiies wrong, I did not consider the heap push and pop. Also, space was also wrong. min(m,n) is for BFS wavefront.
- I did not need a visited marker.

My reasoning to reach Dijkstra was, we are updating the value of a node's distance somewhere in the path. 

But in Dijkstra, when we reach a node twice, it is possible, due to non-uniform weights that we can later reach to the nodes with a shorter distance.
In this case, when we revisit, we always reach by a longer path.

## Complexity

Time: O(mxn) 
Space: O(mxn)

## Code

```text
seed: for each cell (r, c) in grid:
    if rooms[r][c] == 0 (gate):
        enqueue (0, r, c)

while queue not empty:
    pop (d, r, c)

    for each neighbor (nr, nc):
        if in bounds AND rooms[nr][nc] == INF:
            rooms[nr][nc] = d + 1
            enqueue (d+1, nr, nc)

// rooms is modified in-place, no separate visited array needed
// rooms[nr][nc] == INF serves as the "unvisited room" check
```
