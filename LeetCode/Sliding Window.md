- Sliding window is the  most powerful for substring. When it comes to subsequence, sliding window is no longer useful. 
- Window uses left closed right open `[left, right)`

---

# Minimum length substrings/subarrays

1. shift end until the window is qualified. (Expand the window)
2. Then shift start until the window is not qualified. (Shrink the window). Record the answer as you shrink the window. 

```cpp
int left = 0, right = 0;

while (right < nums.size()) {
    // expand window
    window.addLast(nums[right]);
    right++;
    
    while (window needs shrink) {
        // shrink window
        window.removeFirst(nums[left]);
        left++;
    }
}
```

[[76. Minimum Window Substring]]

[[567. Permutation in String]]

[[438. Find All Anagrams in a String]]

[[209. Minimum Size Subarray Sum]]

---

# Maximum substrings/subarrays

1. Only shift right when window is valid
2. Shift left when the window is not valid until the window is valid.
3. Update your answer at the end whenever the window is valid. 

## Length

[[3. Longest Substring Without Repeating Characters]]

## Number/Occurence

Sliding window is good at at most, not at least and exact occurences of something. So we use `atMost(k) - atMost(k - 1)` to find exactly k occurences. 

[[159. Longest Substring with At Most Two Distinct Characters]]

[[340. Longest Substring with At Most K Distinct Characters]]

[[1248. Count Number of Nice Subarrays]]

[[992. Subarrays with K Different Integers]]

[[930. Binary Subarrays With Sum]]


---

[[1151. Minimum Swaps to Group All 1's Together]]

[[Minimum Swaps to group all 1s and all 0s together]]

Problems that I did in the past (not recorded).

maximum, shift both left and right. 

- Using deque
	- 1425
	- 239

862

904

1004
