# Shortest Path (Dijkstra, 0-1 BFS): Progressive Questions

**Asked at (2025–26 reports):** Databricks, Google

**Signal:** weighted edges, 'minimum cost / time / effort to reach'.

**Core idea:** Dijkstra = BFS with a min-heap. Use 0-1 BFS when weights are only 0 and 1. Only use Dijkstra when plain BFS can't work (Databricks expected BFS for equal costs).

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 743 Network Delay Time, 787 Cheapest Flights Within K Stops, 778 Swim in Rising Water, 1584 Min Cost to Connect All Points

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1514 Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/) | M | Dijkstra with max instead of min |  |  |
| [1631 Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) | M | Dijkstra on a grid |  |  |
| [1334 Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) | M | Floyd-Warshall |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1976 Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/) | M | Dijkstra + count paths |  |  |
| [2812 Find the Safest Path in a Grid](https://leetcode.com/problems/find-the-safest-path-in-a-grid/) | M | multi-source BFS, then a max-min path |  |  |
| [1368 Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/) | H | 0-1 BFS |  |  |
| [2045 Second Minimum Time to Reach Destination](https://leetcode.com/problems/second-minimum-time-to-reach-destination/) | H | keep the two best times per node |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [882 Reachable Nodes In Subdivided Graph](https://leetcode.com/problems/reachable-nodes-in-subdivided-graph/) | H | Dijkstra, then count edge coverage |  |  |
| [2577 Minimum Time to Visit a Cell In a Grid](https://leetcode.com/problems/minimum-time-to-visit-a-cell-in-a-grid/) | H | waiting by stepping back and forth |  |  |
| [2203 Minimum Weighted Subgraph With the Required Paths](https://leetcode.com/problems/minimum-weighted-subgraph-with-the-required-paths/) | H | three Dijkstras, one on the reversed graph |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
