---
topic: Trie
difficulty: Medium
leetcode: https://leetcode.com/problems/implement-trie-prefix-tree/
---
# Implement Trie (Prefix Tree)

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/implement-trie-prefix-tree/)

> Node = 26 children + is_end. Insert, search and prefix all just walk the chars.

## Notes

Idea is to have another TrieNode class, which will contain a vector of TrieNode ptrs with fixed value as 26(more if needed). Also, a ending marker boolean.

## Complexity

Complexities: O(N). N: length of the biggest string.

## Code

```text
TrieNode:
    children[26], is_end = false

Trie:
    root = new TrieNode()

Insert(word):                          // Time: O(m), Space: O(m)
    node = root
    for each char → create if missing, advance
    mark node as end

Search(word):                          // Time: O(m), Space: O(1)
    node = Traverse(word)
    return node != null AND node.is_end

StartsWith(prefix):                    // Time: O(m), Space: O(1)
    return Traverse(prefix) != null

Traverse(word):                        // Time: O(m), Space: O(1)
    node = root
    for each char → if missing return null, else advance
    return node
```
