# Trees: Progressive Questions

**Asked at (2025–26 reports):** Meta, Apple, Google

**Signal:** parent/child structure, paths, levels, 'distance K', subtrees.

**Core idea:** Decide what each DFS call returns to its parent. Store the global answer separately. Use BFS for levels and widths.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 226, 104, 543, 110, 100, 572, 235, 102, 199, 1448, 98, 230, 105, 124, 297

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [236 Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) | M | the general version, not the BST one |  |  |
| [1110 Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/) | M | post-order; asked at MongoDB |  |  |
| [863 All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/) | M | add parent links, then BFS |  |  |
| [437 Path Sum III](https://leetcode.com/problems/path-sum-iii/) | M | prefix sum on a tree |  |  |
| [662 Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/) | M | BFS with position indexes |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [987 Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/) | H | the free version of Meta's favorite (314 is Premium) |  |  |
| [1372 Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/) | M | return two values per node |  |  |
| [2096 Step-By-Step Directions From a Binary Tree Node to Another](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/) | M | LCA + path strings |  |  |
| [968 Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/) | H | three states per node |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [834 Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/) | H | rerooting with two DFS passes |  |  |
| [2458 Height of Binary Tree After Subtree Removal Queries](https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/) | H | precompute heights and depths |  |  |
| [1028 Recover a Tree From Preorder Traversal](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/) | H | stack by depth |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
