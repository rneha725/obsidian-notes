# Backtracking: Progressive Questions

**Asked at (2025–26 reports):** LinkedIn

**Signal:** 'generate all', permutations, placements, puzzles.

**Core idea:** Choose, explore, un-choose. Prune early, and sort first when you need to skip duplicates.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 78, 39, 40, 46, 90, 79 Word Search, 131, 17 Letter Combinations, 51 N-Queens, 22

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [93 Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/) | M | split into 4 valid parts |  |  |
| [47 Permutations II](https://leetcode.com/problems/permutations-ii/) | M | skip duplicates |  |  |
| [216 Combination Sum III](https://leetcode.com/problems/combination-sum-iii/) | M | small pruning drill |  |  |
| [698 Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) | M | fill buckets; sort descending to prune |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [473 Matchsticks to Square](https://leetcode.com/problems/matchsticks-to-square/) | M | 698 with k = 4 |  |  |
| [37 Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) | H | constraint sets for rows, columns, boxes |  |  |
| [282 Expression Add Operators](https://leetcode.com/problems/expression-add-operators/) | H | track the last operand for * |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [301 Remove Invalid Parentheses](https://leetcode.com/problems/remove-invalid-parentheses/) | H | BFS or backtracking with counts |  |  |
| [1307 Verbal Arithmetic Puzzle](https://leetcode.com/problems/verbal-arithmetic-puzzle/) | H | column-by-column search |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
