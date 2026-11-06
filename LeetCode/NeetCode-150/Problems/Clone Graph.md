---
topic: Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/clone-graph/
---
# Clone Graph

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/clone-graph/)

> BFS with a map from original to clone. Always link the neighbor, even if already visited.

## Notes

It was for no-self-loop, undirected graph. Used BFS to traverse the graph, and then used a hashmap to keep track of the new node and retrieve a node by value as we have unique values for each node. Q has pair of originalNode, clonedNode. We also need to make sure that although for an edge two nodes are visited but they still need to be connected. That translates into alsways paying attention to a nbr, visited or not. Idea is that for each cloneNode, the nbr array should be same as the original one.

## Complexity

Time: O(V + E)
Space: O(V) -> hashmap storage

## Code

```text
cloneGraph(node):
    if node is null → return null

    create clone of node
    map[node] = clone
    queue = [(node, clone)]

    while queue not empty:
        (orig, clone) = dequeue

        for each nbr in orig.neighbors:
            if nbr not in map:
                map[nbr] = new Node(nbr.val)
                enqueue (nbr, map[nbr])

            clone.neighbors.add(map[nbr])

    return map[node]
```
