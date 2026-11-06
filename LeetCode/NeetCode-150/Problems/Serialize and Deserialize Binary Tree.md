---
topic: Trees
difficulty: Hard
leetcode: https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
---
# Serialize and Deserialize Binary Tree

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

> Preorder with null markers. Deserialize by rebuilding recursively from a token queue.

## Problem

Design an algorithm to serialize and deserialize a binary tree.

## Notes

Serialization: Using a pre-ordered traversal which alsa contains markers for null. When pre-order is marked with nulls, it is easier to make up tree from it.
Deserialization: Split the string and push into a queue, and it is again like a pre-order type recursion which creates the tree back.

You can also use a level-order traversal with null markers, running a few example will show how we can deserialize.

General Notes: 
- No use of global variables
- Use of nullptr and not NULL
- In serialize, we are using string concat which creates string copies, we can use string strream which is a buffer.

## Complexity

Complexities: O(N)

## Code

```text
serialize(root):
    if root is null → return "x"
    return root.val + "," + serialize(left) + "," + serialize(right)

deserialize(data):
    split data by "," → queue of tokens
    return buildTree(queue)

buildTree(queue):
    token = queue.pop_front()
    if token == "x" → return null
    node = new Node(token)
    node.left  = buildTree(queue)
    node.right = buildTree(queue)
    return node
```
