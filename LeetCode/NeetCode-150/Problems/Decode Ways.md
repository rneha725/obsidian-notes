---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/decode-ways/
---
# Decode Ways

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/decode-ways/)

> dp[i] = dp[i+1] if s[i] ≠ '0', plus dp[i+2] if the two digits are valid. Watch edge cases.

## Problem

For example, "11106" can be decoded into:

"AAJF" with the grouping (1, 1, 10, 6)
"KJF" with the grouping (11, 10, 6)
The grouping (1, 11, 06) is invalid because "06" is not a valid code (only "6" is valid).
Note: there may be strings that are impossible to decode.

## Notes

I am proud of myself for this solution, I could come up with a bottom up approach faster. I was thinking of using top down and a recursion, but when I tried I could see how easily this can be solvd with a bottom up.

Try to come up with edge cases. That has been a recurrent weak point.
Keep your base cases ready, in this example, you did not look for '12', '26' kind of examples. Also, invalid inputs etc.

For bottom up dp I can see the dp[i] = dp[i + 1] //if s[i] is not zero, if zero then dp[i] = 0
in case we have a two digit, which is a valid character representation, then dp[i] += dp[i+2]

from the above formula we can see that we only need two valriables, next_one and next_two.

likewise this can be done from the start.

## Complexity

TIme: O(n)
Space: O(1)

## Code

```text
next_one = 1, next_two = 0

for i from n-1 to 0:
    curr = 0
    if s[i] != '0':
        curr = next_one
        if valid two-digit s[i..i+1]:
            curr += next_two
    next_two = next_one
    next_one = curr

return next_one
```
