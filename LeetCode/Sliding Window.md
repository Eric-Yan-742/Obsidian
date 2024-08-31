1. Number of substrings/subarrays
    1. `atMost(k) - atMost(k - 1)`
    2. shift end until [start, end] is not qualified
    3. Then shift start until [start, end] is qualified
2. Minimum substrings/subarrays
    1. shift end until [start, end] is qualified
    2. Then shift start until [start, end] is not qualified.
    3. Record minimum length while shifting start
3. Maximum substrings/subarrays
    1. Only shift start when [start, end] is qualified
    2. Shift both start and end when [start, end] is not qualified
    3. `return end - start`

[[1151. Minimum Swaps to Group All 1's Together]]

[[Minimum Swaps to group all 1s and all 0s together]]

  

1248

992

76

1234

1004

930

904

209

  

Deque

862