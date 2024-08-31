- [代码随想录 (programmercarl.com)](https://www.programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- We can bring one item multiple times.

1. Definition: same as [[0-1 Knapsack Problem]]
2. Recurrence Relation: same as [[0-1 Knapsack Problem]]
3. Initialization: same as [[0-1 Knapsack Problem]]
4. Traversal Order: When traversing the same row (capacity), we do left to right instead of right to left. `for(int j = weight[i]; j <= bagWeight; j++)`

- Why do we traverse from left to right? Look at this example
    - Items:
        
        ![[_attachments/Untitled 50.png|Untitled 50.png]]
        
    - Comparison between traversal orders
        
        ![[_attachments/Untitled 19.jpeg|Untitled 19.jpeg]]
        
    - If we traverse from right to left, we only consider the previous row. Grids in the same row don’t affect each other. That’s why we only consider the item for one time. If we traverse from left to right, the grid may be updated by the current item. Thus, we can add one item multiple times.
- For pure complete Knapsack problem, traverse items then bag (row by row) or bag then items (column by column) are both fine. Take the same example.
    
    ![[_attachments/Untitled 1 18.png|Untitled 1 18.png]]
    
    However, column by column doesn’t work for 0/1 knapsack because we have to traverse from right to left. In that case, we don’t have all the data of the last row yet.