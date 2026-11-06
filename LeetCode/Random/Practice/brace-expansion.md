https://leetcode.com/problems/brace-expansion/

- it was a string related question, and I could not find the way to divide the string. Initially I was not sure if I needed to divide the string.
- Once I realised I needed to divide, I still could not have a strategy to divide it iteratively or recursively.
- Once I read the solution, I could see that each brace is taken as an option for the result, and others are just positions. Once we can create a 2d matrix for this, a backtracking solution was enough.


```cpp
//I tried solving it and could not see how this is closely connected to being a tree.

//I could not build the logic for dividing the input even after understanding that it,

//After reading algo monster, I realised that we can use backtracking here by considering that each brace is giving us option to choose, and other characters are just the positional characters

//abc


class Solution {

public:

	vector<vector<string>> components;
	
	vector<string> getCommaSeparatedChars(const string &s, int start, int end) {
		vector<string> res;
		for(int idx = start; idx <= end; idx = idx + 2) {
			res.push_back(string(1, s[idx]));
		}
		return res;
	}

	void getComponents(const string &s, int i) {
	
		if(s[i] == '{') {
			int close_paren = i + 1;
			while(s[close_paren] != '}') close_paren++;
			vector<string> parent_comps = getCommaSeparatedChars(s, i + 1, close_paren - 1);
			sort(parent_comps.begin(), parent_comps.end());
			components.push_back(parent_comps);
			getComponents(s, close_paren + 1);
		} else {
			int open_paren = i + 1;
			while(open_paren < s.size() && s[open_paren] != '{') open_paren++;
			
			for(int idx = i; idx < open_paren; ++idx) {
				vector<string> pos_char;
				pos_char.push_back(string(1, s[idx]));
				components.push_back(pos_char);
			}
	
			if(open_paren < s.size() && s[open_paren] == '{') getComponents(s, open_paren);
		}
	}

	vector<string> backtrack(int i) {
		vector<string> res;
		if(i >= components.size()) {
			res.push_back("");
			return res;
		}
		vector<string> curr_row = components[i];
		vector<string> next_rows = backtrack(i + 1); 
		for(string s: curr_row) {
			for(string nxt: next_rows) {
				res.push_back(s + nxt);
			}
		}
		return res;
	}

	vector<string> expand(string s) {
		getComponents(s, 0);
		vector<string> res = backtrack(0);
		return res;
	}
};
```

### Patterns to practice
- recursive tree building, divide and conquer