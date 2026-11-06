---
topic: Advanced Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/network-delay-time/
---
# Network Delay Time

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/network-delay-time/)

> Dijkstra. Skip stale heap entries. Answer = largest distance.

## Problem

You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

## Notes

This was easy to see that it was a Dijkstra, I stuck on one part that it requires me to create a weighted graph, for these make this type of class
using Pair = pair<int, int>;
class WeightedGraph { //node: [nbr, time], [nbr2, time2]
 private:
 vector<vector<Pair>> graph_;
 int n_;

 public:
 WeightedGraph(int size) {
 n_ = size;
 graph_ = vector<vector<pair<int, int>>>(n_);
 }

 void CreateGraph(const vector<vector<int>> &data) {
 for(const vector<int> &edge: data) {
 graph_[edge[0] - 1].push_back({edge[1] - 1, edge[2]});
 }
 }

 const vector<Pair>& GetNeighbours(int node_val) const {
 return graph_[node_val];
 }
}; 

- The other thing was optimizing the Dijsktra a bit, in case we pull out from the Q some node which has been calculated before and the curr node's distance is more -> skip this node.

## Complexity

O((V + E) log V); V+ E

## Code

```cpp
void ShortestTime(WeightedGraph &graph, vector<int> &distances, int src) {
        //distance, node
        Min_PQ min_pq;
        distances[src] = 0;
        min_pq.push({distances[src], src});

        while(!min_pq.empty()) {
            auto [curr_distance, curr_node] = min_pq.top(); min_pq.pop();

            if(curr_distance > distances[curr_node]) continue; // note: micro optimization on algo

            //go through the nbrs for this top
            for(auto &[nbr_node, nbr_distance]: graph.GetNeighbours(curr_node)) {  // note:using [] for splitting the values 
                int new_distance = curr_distance + nbr_distance;
                if(new_distance < distances[nbr_node]) {
                    distances[nbr_node] = new_distance;
                    min_pq.push({distances[nbr_node], nbr_node});
                }
            }
        }
    }
```
