---
topic: Advanced Graphs
difficulty: Medium
leetcode: https://leetcode.com/problems/cheapest-flights-within-k-stops/
---
# Cheapest Flights Within K Stops

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

> Bellman-Ford for k+1 rounds, using a copy of distances each round.

## Notes

THis one was really tricky for me, as I thought of solving this using dfs, finding all the paths and then count the hops. But the problem is, one node can update the value of another, dfs does not give me the ease of updating the value of a node which is already in the stack. Or it can be done using dfs, just that I haven't gotten the right algo, why not BFS?

Tried BFS, you need to prune in the BFS with the hops and the price too.

## Complexity

time: k*E; Space: V

## Code

```cpp
class Solution {
public:
    int findCheapestPrice(int n_vertices, vector<vector<int>>& flights, int source, int target_dest, int max_hops) {
        vector<int> distances(n_vertices, numeric_limits<int>::max());
        distances[source] = 0;
        
        for (int i = 0; i <= max_hops; ++i) {
            vector<int> temp = distances;
            for (auto &flight_info: flights) {
                int city = flight_info[0];
                int dest = flight_info[1];
                int price = flight_info[2];
                
                if(distances[city] != numeric_limits<int>::max() && distances[city] + price < temp[dest]) {
                    temp[dest] = distances[city] + price;
                }
            }

            swap(distances, temp);
        }

        return distances[target_dest] == numeric_limits<int>::max() ? -1 : distances[target_dest];
    }
};
```
