- Tiktok OA 25summer Aug 17-21
    
    ![[45ca5eb70b52dcd8e1ade5cbf294558.jpg]]
    
- [Find pair with maximum GCD in an array - GeeksforGeeks](https://www.geeksforgeeks.org/find-pair-maximum-gcd-array/)
- Brute Force: enumerate all the pairs in the array and calculate their gcd one by one. 
    
    ```C++
    #include <bits/stdc++.h>
     
    using namespace std;
     
    // function to find GCD of pair with
    // max GCD in the array
    int findMaxGCD(int arr[], int n)
    {
        int maxGcd = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int gcd = __gcd(arr[i], arr[j]);
                maxGcd = max(maxGcd, gcd);
            }
        }
        return maxGcd;
    }
    ```
    
    `__gcd()` is in the `<algorithm>` header file
    
    Time: $O(n)$﻿
    
    Space: $O(1)$﻿
    
- Use more space in exchange of less time
    
    ```C++
    int findGCD(vector<int>& nums) {
        // largest elements in the array
        int high = 0;
        for (int i = 0; i < nums.size(); i++) {
            high = max(high, nums[i]);
        }
        // record the occurences of distinct divisors of each element. 
        // Space: O(high)
        vector<int> divisors(high + 1, 0);
        for (int i = 0; i < nums.size(); i++) {
            for (int j = 1; j <= sqrt(nums[i]); j++) {
                if (nums[i] % j == 0) {
                    divisors[j]++;
                    // if j^2 != nums[i], we need to record the other half
                    if (j != nums[i] / j) 
                        divisors[nums[i] / j]++;
                }
            }
        }
        // from highest divisor to 1
        for (int i = high; i >= 1; i--) {
            // if any has occurence 2 (or more)
            // it must be the common divisor of at least a pair. 
            if (divisors[i] > 1) return i;
        }
        return 1;
    }
    ```
    
    Time: $O(n * sqrt(nums[i]) + high)$﻿
    
    Space: $O(high)$