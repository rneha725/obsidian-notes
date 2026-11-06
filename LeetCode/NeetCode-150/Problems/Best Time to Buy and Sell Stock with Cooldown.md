---
topic: 2d DP
difficulty: Medium
leetcode: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
---
# Best Time to Buy and Sell Stock with Cooldown

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

> State machine DP: nothing / holding / just sold. Draw the states first.

## Problem

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

## Notes

State maching with dp: This question was quite unclear.
From this question I understand that you need to visualize your states before solving the problem.

https://gist.github.com/rneha725/3caced10e351cc3279f62e8a502edfe0

## Complexity

O(n), Space: O(1)

## Code

```cpp
class Solution {
public:
    static constexpr int nothing_state = 0;
    static constexpr int held_state = 1;
    static constexpr int sell_state = 2;

    int maxProfit(vector<int>& prices) {
        int nothing_value = 0;
        int held_value = 0;
        int sell_value = 0;

        for(int i = prices.size() - 1; i >= 0; --i) {
            int new_nothing_value = max(held_value - prices[i], nothing_value);
            int new_held_value = max(sell_value + prices[i], held_value);
            int new_sell_value = nothing_value;

            nothing_value = new_nothing_value;
            held_value = new_held_value;
            sell_value = new_sell_value;
        }

        return nothing_value;
    }
};
```
