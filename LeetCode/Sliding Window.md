- sliding window is most powerful for substring. When it comes to subsequence, sliding window is no longer useful. 
1. Number of substrings/subarrays
    1. `atMost(k) - atMost(k - 1)`
    2. shift end until [start, end] is not qualified
    3. Then shift start until [start, end] is qualified

---

3. Minimum substrings/subarrays
    1. shift end until the window is qualified. (Expand the window)
    2. Then shift start until the window is not qualified. (Shrink the window)
    3. Record minimum length while shifting start

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

3. Maximum substrings/subarrays
	1. Only shift right when window is valid
	2. Shift both start and end when [start, end] is not qualified
	3. `return end - start`

[[3. Longest Substring Without Repeating Characters]]

[[159. Longest Substring with At Most Two Distinct Characters]]

[[340. Longest Substring with At Most K Distinct Characters]]



---

[[1151. Minimum Swaps to Group All 1's Together]]

[[Minimum Swaps to group all 1s and all 0s together]]

1425