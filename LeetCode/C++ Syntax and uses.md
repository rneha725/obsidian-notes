
### list
```cpp

// Maintaining a list of integers in the descending order, maximum numbers allowed = max
// sorted_numbers can be empty
void addToList(list<int> &sorted_numbers, int new_number, int max) {
	auto it = sorted_numbers.begin();
	while (it != sorted_numbers.end() && *it >= new_number) ++it;
	sorted_numbers.insert(it, new_number);
	if ((int)sorted_numbers.size() > max) sorted_numbers.pop_back();
}
```

As list will only be used for random access, add 
- random removal
- random add
	- list.push_back(number)
- loop

### priority_queue
