# NeetCode 150 — Topic Wise Summary

### Topic: Trees

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Serialize and Deserialize Binary Tree | Hard | Preorder with null markers. Deserialize by rebuilding recursively from a token queue. | [[Problems/Serialize and Deserialize Binary Tree\|Open]] |
| Median of Two Sorted Arrays | Hard | Binary search the cut in the smaller array so both left halves are ≤ both right halves. | [[Problems/Median of Two Sorted Arrays\|Open]] |
| Subtree Of Another Tree | Hard | Compare trees at every node: O(m·n). Rabin-Karp hashing gives O(m+n), skipped. | [[Problems/Subtree Of Another Tree\|Open]] |

### Topic: Trie

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Implement Trie (Prefix Tree) | Medium | Node = 26 children + is_end. Insert, search and prefix all just walk the chars. | [[Problems/Implement Trie (Prefix Tree)\|Open]] |

### Topic: Graphs

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Number of Islands | Medium | For each unvisited '1', BFS to mark the whole island. Count the BFS calls. | [[Problems/Number of Islands\|Open]] |
| Clone Graph | Medium | BFS with a map from original to clone. Always link the neighbor, even if already visited. | [[Problems/Clone Graph\|Open]] |
| Walls and Gates | Medium | Multi-source BFS from all gates. No Dijkstra: every step costs 1. | [[Problems/Walls and Gates\|Open]] |
| Rotting Oranges | Medium | Multi-source BFS from rotten oranges. Each BFS level is one minute. | [[Problems/Rotting Oranges\|Open]] |

### Topic: Sliding Window

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Longest Repeating Character Replacement | Medium | Window is valid while size − maxFreq ≤ k. maxFreq never needs to go down. | [[Problems/Longest Repeating Character Replacement\|Open]] |
| Sliding Window Maximum | Hard | Monotonic deque of candidates. Front is the max; drop expired ones from the front. | [[Problems/Sliding Window Maximum\|Open]] |

### Topic: 1D DP

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Maximum Profit in Job Scheduling | Hard | Sort by start. dp[i] = max(take: profit + dp[next job via lower_bound], skip: dp[i+1]). | [[Problems/Maximum Profit in Job Scheduling\|Open]] |
| Maximum Length of Pair Chain | Medium | Tried as DP, but it is really greedy. See the Greedy version. | [[Problems/Maximum Length of Pair Chain (1D DP)\|Open]] |
| Maximum Earnings From Taxi | Medium | DP on the number line: dp[i] = max(dp[i−1], dp[start] + earning) for rides ending at i. | [[Problems/Maximum Earnings From Taxi\|Open]] |
| Climbing Stairs | Easy | ways[n] = ways[n−1] + ways[n−2]. Only two variables needed. | [[Problems/Climbing Stairs\|Open]] |
| Min Cost Climbing Stairs | Easy | Each step = its cost + min of the previous two. Answer = min of the last two. | [[Problems/Min Cost Climbing Stairs\|Open]] |
| House Robber | Medium | dp[i] = max(dp[i−1], nums[i] + dp[i−2]). | [[Problems/House Robber\|Open]] |
| House Robber II | Medium | Run House Robber twice: once without the last house, once without the first. Take the max. | [[Problems/House Robber II\|Open]] |
| Longest Palindromic Substring | Medium | Not DP. Expand around every center, odd and even. | [[Problems/Longest Palindromic Substring\|Open]] |
| Palindromic Substrings | Medium | Not DP. Expand around every center and count. | [[Problems/Palindromic Substrings\|Open]] |
| Decode Ways | Medium | dp[i] = dp[i+1] if s[i] ≠ '0', plus dp[i+2] if the two digits are valid. Watch edge cases. | [[Problems/Decode Ways\|Open]] |
| Coin Change | Medium | Unbounded knapsack. dp[a] = 1 + min(dp[a − coin]). | [[Problems/Coin Change\|Open]] |
| Maximum Product Subarray | Medium | Track both max and min so far. A negative number swaps them. | [[Problems/Maximum Product Subarray\|Open]] |
| Word Break | Medium | dp[i] = true if a word ends at i and dp[i − len] is true. Trie + memo also works. | [[Problems/Word Break\|Open]] |
| Longest Increasing Subsequence | Medium | Keep a tails array. Append if bigger, else binary search and replace. O(n log n). | [[Problems/Longest Increasing Subsequence (1D DP)\|Open]] |
| Partition Equal Subset Sum | Medium | 0/1 knapsack to reach sum/2. Loop the target backwards. | [[Problems/Partition Equal Subset Sum\|Open]] |

### Topic: 2d DP

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Unique Paths | Medium | One-row DP, or the math answer C(m+n−2, m−1). | [[Problems/Unique Paths\|Open]] |
| Longest Common Subsequence | Medium | Match → diagonal + 1, else max(up, left). Two rows, 1-indexed. | [[Problems/Longest Common Subsequence\|Open]] |
| Best Time to Buy and Sell Stock with Cooldown | Medium | State machine DP: nothing / holding / just sold. Draw the states first. | [[Problems/Best Time to Buy and Sell Stock with Cooldown\|Open]] |
| Maximum Number of Points with Cost | Medium | Left-to-right and right-to-left running max give each cell's best in O(1). | [[Problems/Maximum Number of Points with Cost\|Open]] |
| Coin Change II | Medium | Counting knapsack. Coins in the outer loop, dp[j] += dp[j − coin], dp[0] = 1. | [[Problems/Coin Change II\|Open]] |
| Target Sum | Medium | DP over all reachable sums using an offset. next[j] = dp[j − num] + dp[j + num]. | [[Problems/Target Sum\|Open]] |
| Interleaving String | Medium | dp[i][j] = (s1 char matches and dp[i−1][j]) or (s2 char matches and dp[i][j−1]). | [[Problems/Interleaving String\|Open]] |
| Longest Increasing Path In A Matrix | Hard | Not really DP. A graph problem: DFS + memo on the grid. | [[Problems/Longest Increasing Path In A Matrix\|Open]] |
| Distinct Subsequences | Hard | If chars match: take diagonal + left, else just left. | [[Problems/Distinct Subsequences\|Open]] |
| Edit Distance | Medium | min(insert, delete, replace) from the three neighbors. One row of space. | [[Problems/Edit Distance\|Open]] |
| Burst Balloons | Hard | Interval DP. Pick the balloon burst LAST in [l, r]. Pad with 1s at both ends. | [[Problems/Burst Balloons\|Open]] |

### Topic: Greedy

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Maximum Subarray | Medium | Kadane: extend the current run, or restart at this element. | [[Problems/Maximum Subarray (Greedy)\|Open]] |
| Maximum Length of Pair Chain | Medium | Sort by end. Take a pair if its start > last end. | [[Problems/Maximum Length of Pair Chain (Greedy)\|Open]] |
| Jump Game | Medium | Track the farthest reachable index. Fail if i ever passes it. | [[Problems/Jump Game\|Open]] |
| Jump Game II | Medium | Count a jump each time i passes the current window's end. | [[Problems/Jump Game II\|Open]] |
| Gas Station | Medium | Total gas < total cost → -1. Else restart at i+1 whenever the running tank goes negative. | [[Problems/Gas Station\|Open]] |
| Merge Triplets To Form Target Triple | Medium | Skip triplets that exceed the target anywhere. Max the rest and compare. | [[Problems/Merge Triplets To Form Target Triple\|Open]] |
| Partition Labels | Medium | Store each char's last index. Extend the part to the furthest last index seen. | [[Problems/Partition Labels\|Open]] |

### Topic: Prefix Sum

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Maximum Subarray | Medium | Best = current prefix − smallest prefix seen before it. | [[Problems/Maximum Subarray (Prefix Sum)\|Open]] |

### Topic: Puzzles

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Maximum Product Subarray | Medium/Hard | Track both max and min so far. A negative number swaps them. | [[Problems/Maximum Product Subarray\|Open]] |
| Longest Increasing Subsequence | Medium | Patience sorting view of the O(n log n) LIS. | [[Problems/Longest Increasing Subsequence (Puzzles)\|Open]] |

### Topic: Linked List

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Add Two Numbers |  | No notes yet. | [[Problems/Add Two Numbers\|Open]] |

### Topic: Intervals

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Insert Interval | Medium | Three phases: add intervals before, merge the overlapping ones, add the rest. | [[Problems/Insert Interval\|Open]] |

### Topic: Stack - Too easy, there are better questions

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Min Stack | Easy | Store (value, current min) pairs. | [[Problems/Min Stack\|Open]] |
| Daily Temperatures | Medium | Monotonic decreasing stack of indices. Pop when a warmer day arrives. | [[Problems/Daily Temperatures\|Open]] |
| Largest Rectangle in Histogram | Hard | Increasing stack. On pop: current i is the right boundary, new top is the left. | [[Problems/Largest Rectangle in Histogram\|Open]] |
| Valid Parentheses | Easy | No notes yet. | [[Problems/Valid Parentheses\|Open]] |
| Car Fleet | Medium | Time to target for each car, sorted by position. A car that catches up joins the fleet ahead. | [[Problems/Car Fleet\|Open]] |

### Topic: Heap

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Task Scheduler | Medium | Max-heap per cycle, or math: max((maxFreq−1)(n+1) + countOfMax, tasks). | [[Problems/Task Scheduler\|Open]] |
| Design Twitter | Medium | Per-user tweet lists + follow set. Merge recent tweets with a size-10 min-heap. | [[Problems/Design Twitter\|Open]] |

### Topic: Backtracking

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Subsets | Medium | Include or exclude each element. Or use a bitmask over 2^n. | [[Problems/Subsets\|Open]] |
| Permutations | Medium | Swap each element into the current index, then recurse. | [[Problems/Permutations\|Open]] |
| Subsets II | Medium | Sort. Skip the same value at the same tree level. Retry this one. | [[Problems/Subsets II\|Open]] |
| Generate Parentheses | Medium | Add '(' if any left; add ')' if close > open. Backtracking, not DP. | [[Problems/Generate Parentheses\|Open]] |
| Combination Sum II | Medium | Sort. Skip duplicates at the same level. Each number used once. | [[Problems/Combination Sum II\|Open]] |
| Word Search | Medium | DFS from each cell. Mark visited in place and restore after. | [[Problems/Word Search\|Open]] |
| Palindrome Partitioning | Medium | Precompute palindromes starting at each index, then backtrack over them. | [[Problems/Palindrome Partitioning\|Open]] |
| Letter Combinations Of A Phone Number | Medium | Easy backtracking over digit → letters. | [[Problems/Letter Combinations Of A Phone Number\|Open]] |
| N Queens | hard | Go row by row. Track attacked cells: place, recurse, undo. | [[Problems/N Queens\|Open]] |

### Topic: Advanced Graphs

| Problem | Difficulty | Notes | Explanation |
|---|---|---|---|
| Network Delay Time | Medium | Dijkstra. Skip stale heap entries. Answer = largest distance. | [[Problems/Network Delay Time\|Open]] |
| Cheapest Flights Within K Stops | Medium | Bellman-Ford for k+1 rounds, using a copy of distances each round. | [[Problems/Cheapest Flights Within K Stops\|Open]] |
