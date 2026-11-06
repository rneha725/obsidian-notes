# Topological Sort: Progressive Questions

**Asked at (2025–26 reports):** LinkedIn, Google, Salesforce

**Signal:** dependencies, build order, prerequisites, 'must come before'.

**Core idea:** Kahn's algorithm: start with nodes that have in-degree 0. If you can't process every node, there's a cycle.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 207 Course Schedule, 210 Course Schedule II

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [2115 Find All Possible Recipes from Given Supplies](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/) | M | build order with supplies. Close to LinkedIn's package build order. |  |  |
| [802 Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/) | M | topo sort on the reversed graph |  |  |
| [1462 Course Schedule IV](https://leetcode.com/problems/course-schedule-iv/) | M | carry prerequisite sets along the order |  |  |
| [310 Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/) | M | peel leaves layer by layer |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [444 Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/) | M (P) | the order is unique only if the queue never holds 2 nodes. Google asked sequence ordering. |  |  |
| [851 Loud and Rich](https://leetcode.com/problems/loud-and-rich/) | M | propagate the best answer in topo order |  |  |
| [2050 Parallel Courses III](https://leetcode.com/problems/parallel-courses-iii/) | H | longest path in a DAG |  |  |
| [1857 Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/) | H | topo order + DP per color |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1203 Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/) | H | two-level topo sort |  |  |
| [269 Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) | H (P) | build edges from neighboring words |  |  |
| [2392 Build a Matrix With Conditions](https://leetcode.com/problems/build-a-matrix-with-conditions/) | H | two separate topo sorts |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
