---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/generate-parentheses/
---
# Generate Parentheses

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/generate-parentheses/)

> Add '(' if any left; add ')' if close > open. Backtracking, not DP.

## Problem

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

## Notes

I thought on this question, on other BT questions too, that why not DP when we are repeating states, but for this question I got to know that DP will tke more time than BT. There is somethign called Catalan number, which increases almost exponentially. and the number of solutions will increase like that. so saving state in bottom up manner won't work.

Other thing is, here we want all answers, not like dp where we want an optimized ansnwer. This is a very important distinction

After this, it was an easy problem.

## Code

```cpp
void BackTrackForParenthesis(int open, int close, vector<string> &res, string &curr, int cap) {
        if(open < 0 | open > close | close < 0) return;
        if(open == 0 && close == 0) {
            res.push_back(curr);
            return;
        }

        //open
        curr.push_back('(');
        BackTrackForParenthesis(open - 1, close, res, curr, cap);
        curr.pop_back();

        //close
        curr.push_back(')');
        BackTrackForParenthesis(open, close - 1, res, curr, cap);
        curr.pop_back();
    }
```
