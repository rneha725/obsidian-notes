---
topic: Sliding Window
difficulty: Medium
leetcode: https://leetcode.com/problems/longest-repeating-character-replacement/
---
# Longest Repeating Character Replacement

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/longest-repeating-character-replacement/)

> Window is valid while size − maxFreq ≤ k. maxFreq never needs to go down.

## Problem

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Notes

A tricky one. It is easy to see it is a sliding window.

I thought of going through each character, use sliding window for a fixed character and find out the max window size possible. This is a good solution, O(26*n). A one-pass optimization is to use a trick.

Crux of the trick is this formula: windowSize - maxFreq <= k

Initially I thought of going about a sliding window and knowing what is the max freq in each window and then get the answer. But the trick is that we are concerned with a global max window length.
The algorithm doesn't care about finding the exact answer at every step.
It only cares: "Have I seen a valid window larger than my current best?"

So, what we can do is, we can shrink the window only when windowSize - maxFreq > k.

For this maxFreq,we know that we cannot achieve a bigger window size, as it will always be maxFreq + k
So, now we need to find the next max Freq. This helps us do this problem in O(n) time.

## Code

```text
function characterReplacement(s, k):
    freq[26] = {0}
    max_freq = 0
    left = 0
    res = 0

    for right in 0..len(s)-1:
        freq[s[right]]++
        max_freq = max(max_freq, freq[s[right]])

        window_size = right - left + 1
        replacements_needed = window_size - max_freq

        if replacements_needed > k:
            freq[s[left]]--
            left++
        else:
            res = max(res, window_size)

    return res
```
