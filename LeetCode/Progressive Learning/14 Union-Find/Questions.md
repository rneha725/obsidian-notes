# Union-Find: Progressive Questions

**Asked at (2025–26 reports):** general senior loops (grouping and connectivity)

**Signal:** merge groups, 'are these connected', grouping by equality.

**Core idea:** find with path compression, union by rank. Weighted union-find stores a ratio on each edge.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 684 Redundant Connection, 323 / 261 (P)

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [547 Number of Provinces](https://leetcode.com/problems/number-of-provinces/) | M | plain union-find |  |  |
| [990 Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations/) | M | union the ==, then check the != |  |  |
| [721 Accounts Merge](https://leetcode.com/problems/accounts-merge/) | M | union by email |  |  |
| [1202 Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/) | M | group indexes, sort each group |  |  |
| [947 Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/) | M | union rows with columns |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [399 Evaluate Division](https://leetcode.com/problems/evaluate-division/) | M | weighted union-find |  |  |
| [959 Regions Cut By Slashes](https://leetcode.com/problems/regions-cut-by-slashes/) | M | split each cell into 4 triangles |  |  |
| [2092 Find All People With Secret](https://leetcode.com/problems/find-all-people-with-secret/) | H | union per time step, then reset |  |  |
| [1579 Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/) | H | two union-finds |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1697 Checking Existence of Edge Length Limited Paths](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/) | H | offline queries sorted by limit |  |  |
| [685 Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/) | H | directed version: handle the two-parent case |  |  |
| [803 Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit/) | H | reverse time: add bricks back |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
