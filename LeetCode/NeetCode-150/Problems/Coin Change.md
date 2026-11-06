---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/coin-change/
---
# Coin Change

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/coin-change/)

> Unbounded knapsack. dp[a] = 1 + min(dp[a − coin]).

## Problem

This problem hates me.

Select as many as coins, infinitely. Return minimum number of coins we need to make the target amount.

## Notes

unbounded knapsack

Bad:
I tried top down but I think I was still facing the issue getting to the way it should be solved.

Dp problems are solved with subproblems. What subproblems? given and array, and an amount, we need to find the min coins.
dp[am] = 1 + min(dp[am - all_coins]). 
E.g [1,2,5], 11 -> dp[11] = 1 + min(dp[10], dp[9], dp[6])

I also realised my way of solving top down dp prblems are wrong, I accumulate the result at the leaf, which is more backtraacking. Correct way of memoized dp is to let the child return value to the parent, and let the parent do whatever it needs to with the retuned value.

solve(i):
 if i == n:
 return 0 ← base case
 return arr[i] + solve(i+1) ← parent combines
This is standard DP / memoization

memoization caches the return value of solve(i). In Style 1, solve(i, current_sum) has the running sum baked into the arguments — two calls to the same i with different current_sum are treated as different states, so you get no cache hits. Memo is useless.

Now coming onto the solution, using bottom up. 
We see this: dp[am] = 1 + min(dp[am - all_coins]). 

and exactly this is what we need to use, start with the min amount possible and keep on increasing it, the biggest thing is to always find this type of formula dependant on the subproblems.

## Complexity

Time: O(am*coins_len)
Space: O(am)

## Code

```cpp
vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;

        for(int i = 1; i <= amount; ++i) {
            for(int coin: coins) {
                if(i - coin >= 0 && dp[i - coin] != INT_MAX) {
                    dp[i] = min(dp[i], 1 + dp[i - coin]);
                }
            }
        }

        return dp[amount] == INT_MAX ? -1 : dp[amount];
```
