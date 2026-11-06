# Graphs (BFS / DFS): Progressive Questions

**Asked at (2025–26 reports):** Google, Meta, Databricks, LinkedIn, Salesforce, Booking, Twilio

**Signal:** a grid, a network, 'fewest steps', 'connected regions', 'can you reach'.

**Core idea:** BFS for shortest steps when every move costs the same. DFS for regions and reachability. Add extra state (keys, eliminations) to the visited set when the problem has it.

**Already solved in NeetCode 150 (redo only if rusty):** 200 Number of Islands, 133 Clone Graph, 286 Walls and Gates, 994 Rotting Oranges, 329 Longest Increasing Path in a Matrix

## Stage 1: Core (Medium)

| Problem                                                                                              | Level | Hint                                          | Status | My Notes                       | Link                         |
| ---------------------------------------------------------------------------------------------------- | ----- | --------------------------------------------- | ------ | ------------------------------ | ---------------------------- |
| [1254 Number of Closed Islands      ](https://leetcode.com/problems/number-of-closed-islands/)       | M     | DFS from the border first. Asked at Google.   |        | Flood fill algorithm - use dfs | [[Number of Closed Islands]] |
| [1020 Number of Enclaves            ](https://leetcode.com/problems/number-of-enclaves/)             | M     | same border trick, count cells                |        |                                |                              |
| [785 Is Graph Bipartite?            ](https://leetcode.com/problems/is-graph-bipartite/)             | M     | BFS with two colors                           |        |                                |                              |
| [1091 Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/) | M     | plain BFS, 8 directions                       |        |                                |                              |
| [934 Shortest Bridge                ](https://leetcode.com/problems/shortest-bridge/)                | M     | DFS to find one island, then multi-source BFS |        |                                |                              |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1162 As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/) | M | multi-source BFS from all land |  |  |
| [127 Word Ladder](https://leetcode.com/problems/word-ladder/) | H | BFS on words. LinkedIn asked word transformations. |  |  |
| [827 Making A Large Island](https://leetcode.com/problems/making-a-large-island/) | H | label islands with sizes, then try each 0. Asked at Meta. |  |  |
| [1293 Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/) | H | BFS state = (row, col, eliminations left) |  |  |
| [864 Shortest Path to Get All Keys](https://leetcode.com/problems/shortest-path-to-get-all-keys/) | H | BFS state includes a bitmask of keys |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [2290 Minimum Obstacle Removal to Reach Corner](https://leetcode.com/problems/minimum-obstacle-removal-to-reach-corner/) | H | 0-1 BFS with a deque |  |  |
| [773 Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/) | H | BFS over board states |  |  |
| [1036 Escape a Large Maze](https://leetcode.com/problems/escape-a-large-maze/) | H | huge grid: BFS with a step limit. Google has asked infinite-grid questions. |  |  |
| [126 Word Ladder II](https://leetcode.com/problems/word-ladder-ii/) | H | BFS for distances, then DFS to build paths |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.

### Implementation Bugs
##### Frequent
- Check bfs/dfs condition on when you are pushing a value. You sometimes miss visited value or other filters.
