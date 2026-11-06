---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/coin-change-ii/
---
# Coin Change II

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/coin-change-ii/)

> Counting knapsack. Coins in the outer loop, dp[j] += dp[j − coin], dp[0] = 1.

## Problem

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

## Notes

Unbounded knapsack.: Honestly, do Coind Change first.

This problem was extremely difficult when I started fresh, then for some random reason I retried coin. change and the solution came really really easy for this one.

I can talk a out the crux but a bigger part of the solution is to actually solve coin change.

From there, it is derived that:

dp[i] = dp[i] + dp[i - curr_coin]

seed dp[0] = 1

## Complexity

T(n*amount); S(amount)

## Code

```cpp
int change(int amount, vector<int>& coins) {
        vector<unsigned long long> dp(amount + 1, 0);
        dp[0] = 1;
        
        for(int i = 0; i < coins.size(); ++i) {
            for(int j = coins[i]; j <= amount; ++j) {
                dp[j] = dp[j] + dp[j - coins[i]];
            }
        }

        return (int)dp[amount];
    }
```
