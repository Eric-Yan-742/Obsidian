- [题目页面 (kamacoder.com)](https://kamacoder.com/problempage.php?pid=1046)
- For this type of problem, each type has only one item. Thus, 0 and 1 mean
    - 0: don’t bring this item
    - 1: bring this item
- N types of objects. Each type has one. Each type has corresponding value. Specified how many types of objects we can carry. How do we achieve maximum value?

- Brute Force: Backtracking
    
    Try all possible options that
    
    ```C++
    int backtracking(int i, int m, int n,vector<int>& value, vector<int>& weight, 
            int totalValue, int totalWeight) {
        // if i==m, it means we've checked all items in this branch
        // one branch has m items. Each item has binary options bring/not bring
        if(i >= m) return totalValue;
        int hasValue = 0;
        int noValue = 0;
        // Prunning: We can only bring this item if weight doesn't exceed. 
        if(totalWeight + weight[i] <= n) {
            hasValue = backtracking(i + 1, m, n, value, weight, totalValue + value[i], 
		            totalWeight + weight[i]);
        }
        noValue = backtracking(i + 1, m, n, value, weight, totalValue, totalWeight);
        return max(hasValue, noValue);
    }
    int main() {
        // getting input. 
        int m,n;
        cin >> m >> n;
        vector<int> weight(m,0);
        vector<int> value(m,0);
        for (int i = 0; i < m; i++) {
            cin >> weight[i];
        }
        for (int j = 0; j < m; j++) {
            cin >> value[j];
        }
        // same as return the result
        cout<<backtracking(0, m, n, value, weight, 0, 0)<<endl;
    }
    ```
    
    `i` means that we’re checking if we can add item i, we haven’t added it yet.
    
- 2D dp
    
    - `dp[i][j]` : `dp[i][j]` is the maximum value we can get considering objects `bags[0 : i]` with volume j.
    - Recurrence Relation
        
        1. If don’t bring item `i` , max value is `dp[i - 1][j]`
        2. If bring `i` , max value is `dp[i - 1][j - weight[i]] + value[i]` . `j - weight[i]` because `j` has to be able to contain object `i` .
        
        - `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])` .
    - Initialization: This initialization follows the definition. 
        - `dp[i - 1][j]` is the grid right above, `dp[i - 1][j - weight[i]]` is all the grids on the left of `dp[i - 1][j]` at the same row. Thus, we need to initialize the first row and first column.
            
            ![[_attachments/Untitled 48.png|Untitled 48.png]]
            
            ![[_attachments/Untitled 1 17.png|Untitled 1 17.png]]
            
        - For the first row, `i == 0` so we can only choose object `0` . If bag cannot contain object `0` , max value should be 0.
        - For the first column, because `j == 0` , bag cannot contain anything, so max values are all 0.
    - Traversal Order: row by row or column by column are both fine because both cases will not have gaps.
    
    row by row version:
    
    ```C++
    // total of m-1 objects, n volumes
    vector<vector<int>> dp(m, vector<int>(bagVolume + 1, 0));
    // initialize first column, all 0
    for(int i = 0; i < m; i++) {
        dp[i][0] = 0;
    }
    // initialize first row, j = weight[0] is the key
    for(int j = weight[0]; j <= bagVolume; j++) {
        dp[0][j] = value[0];
    }
    // row by row
    for(int i = 1; i < m; i++) {
        for(int j = 1; j <= bagVolume; j++) {
            // if bag cannot contain this item, we can only leave this item.
            if(j < weight[i]) {
                dp[i][j] = dp[i - 1][j];
            // if we can take this item, get the max value of taking or not taking
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
            }
        }
    }
    
    cout<<dp[m - 1][bagVolume]<<endl;
    ```
    
    Time: $O(m*n)$﻿ Space: $O(m*n)$﻿
    
# 1D dp

Because we just need the last row，we can use a n-element array and traverse m times to save space.

1. `dp[j]` means that the max value of a `bag[0 : i]` with `j` capacity
2. Recurrence Relation: `dp[j] = max(dp[j], dp[j - weight[i]] + value[i])`
	- compared to the 2D version, the `dp[j]` in the max is like `dp[j]` in the last row.
3. Initialization: Explicit and Implicit see below. 
4. Traversal: Items then volume. For volume (`j`) we need to **traverse backward**. If we traverse forward, we will use grids that have been updated for this row. If we traverse backward, we will only use old data from last row.

Explicit Initialization: For `objects[0]`, initialize the first row according to the definition. Just like the 2D version although we traverse backward. 

```cpp
vector<int> dp(bagVolume + 1, 0);
for(int i = bagVolume; i >= weight[0] ; i++) {
	dp[i] = value[0];
}
for(int i = 0; i < m; i++) {
	// traverse volumes backward
	// note we use >=weight[i] in case that this volume cannot hold ith item
	for(int j = bagVolume; j >= weight[i]; j--) {
		dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
	}
}
cout<<dp[bagVolume]<<endl;
```

Implicit Intialization: We do the initialization within the recurrence process

```cpp
vector<int> dp(bagVolume + 1, 0);
for(int i = 0; i < m; i++) {
	// Initialize when i == 0
	for(int j = bagVolume; j >= weight[i]; j--) {
		dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
	}
}
cout<<dp[bagVolume]<<endl;
```

Note we use a trick `>= weight[i]` , in this case we know this volume cannot hold item `i` , so we just don’t update it. Just like we copy the data from last row in 2D version.

Time: $O(m * n)$﻿ Space: $O(n)$