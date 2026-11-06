https://leetcode.com/problems/longest-string-chain/

## Approach 1: Graph + DFS with Memo (my solution) -> bad solution

### Code
```cpp
class Solution {
public:
    bool checkIfNeighbours(string &s1, string &s2) {
        if (s1.size() + 1 != s2.size()) return false;
        int mismatch = 0;
        int p1 = 0, p2 = 0;
        for (; p1 < s1.size();) {
            if (s1[p1] != s2[p2]) {
                mismatch++;
                p2++;
            } else {
                p2++;
                p1++;
            }
            if (mismatch > 1) return false;
        }
        return true;
    }

    unordered_map<int, vector<int>> createGraph(vector<string>& words,
                                                unordered_map<int, vector<int>>& length_map,
                                                int mn, int mx) {
        unordered_map<int, vector<int>> graph;
        for (int i = mn; i <= mx; ++i) {
            if (length_map.find(i) != length_map.end() &&
                length_map.find(i + 1) != length_map.end()) {
                vector<int> first_list = length_map[i];
                vector<int> next_list = length_map[i + 1];

                for (int j = 0; j < first_list.size(); ++j) {
                    for (int k = 0; k < next_list.size(); ++k) {
                        if (checkIfNeighbours(words[first_list[j]], words[next_list[k]])) {
                            graph[first_list[j]].push_back(next_list[k]);
                        }
                    }
                }
            }
        }
        return graph;
    }

    int calculateMaxPathLength(int i, unordered_map<int, vector<int>> &graph,
                               vector<bool> &calculated, vector<int> &max_seq_length) {
        if (calculated[i] == true || graph[i].size() == 0) {
            calculated[i] = true;
            return max_seq_length[i];
        }
        for (int nbr : graph[i]) {
            max_seq_length[i] = max(max_seq_length[i],
                                    1 + calculateMaxPathLength(nbr, graph, calculated, max_seq_length));
        }
        calculated[i] = true;
        return max_seq_length[i];
    }

    int longestStrChain(vector<string>& words) {
        int n = words.size();
        int min_len_string = 20, max_len_string = 1;
        unordered_map<int, vector<int>> length_map;

        for (int i = 0; i < words.size(); ++i) {
            int length = words[i].size();
            min_len_string = min(min_len_string, length);
            max_len_string = max(max_len_string, length);
            length_map[length].push_back(i);
        }

        unordered_map<int, vector<int>> graph =
            createGraph(words, length_map, min_len_string, max_len_string);
        vector<int> max_seq_length(n, 1);
        vector<bool> calculated(n, false);
        int res = 1;

        for (int i = 0; i < words.size(); ++i) {
            if (calculated[i] == false) {
                max_seq_length[i] = calculateMaxPathLength(i, graph, calculated, max_seq_length);
                res = max(res, max_seq_length[i]);
            }
        }
        return res;
    }
};
```

---

### Solution Using dp
```cpp
class Solution {
	public:
	int longestStrChain(vector<string>& words) {
		sort(words.begin(), words.end(), [](const string& a, const string& b) {
			return a.size() < b.size();
		});
		
		unordered_map<string, int> dp;
		int res = 0;
		for(int i = 0; i < words.size(); ++i) {
			dp[words[i]] = 1;
			for(int part = 0; part < words[i].size(); ++part) {
				string prev = words[i].substr(0, part) + words[i].substr(part + 1);
				if(dp.find(prev) != dp.end()) {
					dp[words[i]] = max(dp[words[i]], 1 + dp[prev]);
				}
			}
		}
		res = max(res, dp[words[i]]);
		return res;
	}
};
```