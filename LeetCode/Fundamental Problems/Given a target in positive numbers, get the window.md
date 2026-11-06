Given positive numbers and a target number, find the target window having the sum equal to the target.

```
Array: 1,1,4,2,3

target = 5
```

```cpp
int window_sum = 0;
for(int left = 0, right = 0; right < arr.size(); ++right) {
	window_sum += arr[right];
	while(window_sum > target) {
		window_sum -= arr[left];
		left++;
	}
	if(window_sum == target) //record the window_size, left/right coords, whatever the question requires
}
```