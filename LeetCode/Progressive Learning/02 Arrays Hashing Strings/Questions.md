# Arrays, Hashing & Strings: Progressive Questions

**Asked at (2025–26 reports):** LinkedIn, Twilio, Google, Booking, Meta

**Signal:** counting, 'seen before', subarray sums, string rebuilding.

**Core idea:** A hash map turns a second loop into a lookup. Prefix sum + map counts subarrays with a target sum.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 1 Two Sum, 49 Group Anagrams, 347 Top K Frequent, 238 Product Except Self, 128 Longest Consecutive Sequence, 271 Encode and Decode Strings

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [187 Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/) | M | hash every 10-letter window. LinkedIn asked a variant. |  |  |
| [560 Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | M | prefix sum + count map |  |  |
| [525 Contiguous Array](https://leetcode.com/problems/contiguous-array/) | M | turn 0 into -1, then prefix sum |  |  |
| [974 Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/) | M | prefix sum mod k; watch negatives |  |  |
| [443 String Compression](https://leetcode.com/problems/string-compression/) | M | two pointers, in place |  |  |
| [8 String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) | M | edge cases and overflow. Common at Meta. |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [554 Brick Wall](https://leetcode.com/problems/brick-wall/) | M | count edge positions |  |  |
| [2007 Find Original Array From Doubled Array](https://leetcode.com/problems/find-original-array-from-doubled-array/) | M | sort + count map |  |  |
| [41 First Missing Positive](https://leetcode.com/problems/first-missing-positive/) | H | use the array itself as the hash table |  |  |
| [1074 Number of Submatrices That Sum to Target](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/) | H | 560 on every pair of rows |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [30 Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/) | H | word-sized sliding windows + counts |  |  |
| [1316 Distinct Echo Substrings](https://leetcode.com/problems/distinct-echo-substrings/) | H | rolling hash |  |  |
| [1638 Count Substrings That Differ by One Character](https://leetcode.com/problems/count-substrings-that-differ-by-one-character/) | M | tricky counting; good speed drill |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
