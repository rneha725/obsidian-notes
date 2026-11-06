# Trie: Progressive Questions

**Asked at (2025–26 reports):** LinkedIn (typeahead appears in system design), Google

**Signal:** prefixes, autocomplete, word dictionaries, max XOR.

**Core idea:** Each node has children and an end flag. Store extra data on nodes (counts, top suggestions) when needed.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 208 Implement Trie, 211 Add and Search Words, 212 Word Search II

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1268 Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) | M | autocomplete basics |  |  |
| [648 Replace Words](https://leetcode.com/problems/replace-words/) | M | shortest matching prefix |  |  |
| [677 Map Sum Pairs](https://leetcode.com/problems/map-sum-pairs/) | M | store sums on nodes |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [421 Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) | M | bit trie |  |  |
| [1032 Stream of Characters](https://leetcode.com/problems/stream-of-characters/) | H | trie of reversed words |  |  |
| [745 Prefix and Suffix Search](https://leetcode.com/problems/prefix-and-suffix-search/) | H | insert suffix#word combinations |  |  |
| [642 Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/) | H (P) | trie + top-3 by frequency |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [472 Concatenated Words](https://leetcode.com/problems/concatenated-words/) | H | trie + DP |  |  |
| [336 Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/) | H | trie of reversed words + palindrome checks |  |  |
| [1707 Maximum XOR With an Element From Array](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/) | H | offline queries + bit trie |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
