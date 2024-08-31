- Backtraking appears under/after recursion
- Search using backtracking is brute force. But certain problems can only be solved by backtracking
    - Combination problem: Find all combinations of 1234 with size 2 (12, 13, 23…)
    - Cutting: Given a string, how many ways are there to cut this string so that all substrings are…
    - Subset: Find all subsets of 1234
    - Permutation
    - Chessboard
- It’s difficult to use loops to solve these problems.
- How to comprehend backtrackaing algorithm
    - Consider it to be a **tree** structure
    - Width of the tree is number of subsets we are processing
    - Depth of the tree is depth of recursion
- Template
    
    ```Plain
    void backtracking (parameters)
    	if(base case) {
    		collect result
    		return;
    	}
    	for(sets) {
    		process nodes
    		recursion
    		backtracking
    	}
    	return;
    }
    ```