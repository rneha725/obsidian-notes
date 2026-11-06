**Asked at (2025–26 reports):** Airbnb, DocuSign, Google

**Signal:** count ways, min / max cost, choices that overlap.

**Core idea:** Say the state in one sentence first. Then the transition, base case and fill order.

**Already solved in NeetCode 150 (redo only if rusty):** about 25 DP problems (1D and 2D sections in your NeetCode folder)

## Stage 1: Core (Medium)

| Problem                                                                                                    | Level | Hint                      | Status | My Notes                                                                                                                                        | Link                                                    |     |
| ---------------------------------------------------------------------------------------------------------- | ----- | ------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | --- |
| [2140 Solving Questions With Brainpower](https://leetcode.com/problems/solving-questions-with-brainpower/) | M     | take or skip with a jump  | Done   | Using the formula for 1d dp worked well.                                                                                                        | [[[1D DP]Solving Questions with Brainpower]]            |     |
| [1043 Partition Array for Maximum Sum](https://leetcode.com/problems/partition-array-for-maximum-sum/)     | M     | partition DP              | Done   | It is a 2d dp, a bit tricky but get to the formula for tabulation, ith state answers for i to n. Optimize for memory.                           | [[[1D DP - partition] Partition Array for Maximum Sum]] |     |
| [1048 Longest String Chain](https://leetcode.com/problems/longest-string-chain/)                           | M     | LIS on words              | Done   | I saw a graph like structure, but the thing is the the edges are simple connections, it could have been done only using DP. It is not even LIS. | [[[1D DP] Longest String Chain]]]                       |     |
| [474 Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)                                      | M     | knapsack with two budgets |        |                                                                                                                                                 |                                                         |     |
|                                                                                                            |       |                           |        |                                                                                                                                                 |                                                         |     |

## Stage 2: Twists (Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [2328 Number of Increasing Paths in a Grid](https://leetcode.com/problems/number-of-increasing-paths-in-a-grid/) | H | DP on a DAG. Airbnb asked DAG path scoring. |  |  |
| [1335 Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/) | H | exactly d parts |  |  |
| [1751 Maximum Number of Events That Can Be Attended II](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended-ii/) | H | your job-scheduling DP + at most k |  |  |
| [1473 Paint House III](https://leetcode.com/problems/paint-house-iii/) | H | 3D state |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1547 Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/) | H | interval DP |  |  |
| [1092 Shortest Common Supersequence](https://leetcode.com/problems/shortest-common-supersequence/) | H | LCS, then rebuild the string |  |  |
| [1000 Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/) | H | interval DP with an extra state |  |  |

## Pattern Practice

One pattern at a time. Before coding, say the state out loud.

### Tier 1: Must be fluent

Solve each in about 25 minutes. Go top to bottom in each table: easier first.

#### 1. Linear DP

State: `dp[i]` = answer for the first i items

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [740 Delete and Earn](https://leetcode.com/problems/delete-and-earn/) | M | House Robber in disguise |  |  |
| [2369 Check if There is a Valid Partition For The Array](https://leetcode.com/problems/check-if-there-is-a-valid-partition-for-the-array/) | M | look back 2 or 3 steps |  |  |
| [1416 Restore The Array](https://leetcode.com/problems/restore-the-array/) | H | Decode Ways with bigger numbers |  |  |

#### 2. Two-Sequence DP

State: `dp[i][j]` = first i of A, first j of B

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [583 Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | M | LCS in disguise |  |  |
| [712 Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/) | M | LCS with costs |  |  |
| [1458 Max Dot Product of Two Subsequences](https://leetcode.com/problems/max-dot-product-of-two-subsequences/) | H | careful: must pick at least one pair |  |  |

#### 3. Knapsack DP

State: `dp[i][target]`. 0/1 = loop backwards, unbounded = loop forwards

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1049 Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/) | M | 0/1: split into two groups |  |  |
| [377 Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) | M | unbounded, but order matters |  |  |
| [879 Profitable Schemes](https://leetcode.com/problems/profitable-schemes/) | H | 0/1 with two budgets, count ways |  |  |

#### 4. Grid DP

State: `dp[r][c]` = best way to reach this cell

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [931 Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/) | M | come from 3 cells above |  |  |
| [221 Maximal Square](https://leetcode.com/problems/maximal-square/) | M | min of 3 neighbours + 1 |  |  |
| [174 Dungeon Game](https://leetcode.com/problems/dungeon-game/) | H | fill from bottom-right |  |  |

#### 5. LIS Family

State: `dp[i]` = best sequence ending at i

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [646 Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain/) | M | sort, then LIS |  |  |
| [673 Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/) | M | track length and count |  |  |
| [354 Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/) | H | sort trick + binary search LIS |  |  |

#### 6. Interval DP

State: `dp[l][r]`, try every split k, loop by length

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [516 Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | M | ends match or drop one end |  |  |
| [1039 Minimum Score Triangulation of Polygon](https://leetcode.com/problems/minimum-score-triangulation-of-polygon/) | M | pick the third corner k |  |  |
| [664 Strange Printer](https://leetcode.com/problems/strange-printer/) | H | merge same-colour ends |  |  |

### Tier 2: Should be comfortable

Less common, but fair game at L5.

#### 7. State Machine DP

State: `dp[i][state]`, follow the arrows between states

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [714 Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) | M | 2 states: hold, free |  |  |
| [123 Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) | H | 4 states |  |  |
| [188 Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) | H | states x k transactions |  |  |

#### 8. Tree DP

State: DFS returns a small tuple to the parent

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [337 House Robber III](https://leetcode.com/problems/house-robber-iii/) | M | return (rob, skip) |  |  |
| [2246 Longest Path With Different Adjacent Characters](https://leetcode.com/problems/longest-path-with-different-adjacent-characters/) | H | top 2 child paths |  |  |
| [968 Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/) | H | 3 states per node |  |  |

#### 9. DP on DAG / Memo DFS

State: memoized DFS or topological order

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [2050 Parallel Courses III](https://leetcode.com/problems/parallel-courses-iii/) | H | longest path in a DAG |  |  |
| [1857 Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/) | H | topo sort + dp[node][colour] |  |  |

#### 10. Bitmask DP

State: `dp[mask]`, n is 20 or less

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [526 Beautiful Arrangement](https://leetcode.com/problems/beautiful-arrangement/) | M | mask of used numbers |  |  |
| [1986 Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/) | M | mask + time left |  |  |
| [847 Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) | H | BFS over (node, mask) |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.

### Gaps
- Simplifying tabulation is a big problem till now.
	- See if you are carrying more in data as you need
	- The calculation you are nesting inside a dp\[i\]\[j\] cal is it needed?
	- Adding sentinel can help with the boundaries and make the code simpler.
- I struggle with graph and dp complexities.
