- [Quick Sort Algorithm - GeeksforGeeks](https://www.geeksforgeeks.org/quick-sort-algorithm/)
- Recursion
    
    ```C++
    // current interval: [low, high]
    void quickSort(vector<int>& arr, int low, int high) {
        if (low < high) {
          
            // pi is the partition return index of pivot
            int pi = partition(arr, low, high);
    
            // Recursion calls for smaller elements
            // and greater or equals elements
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }
    ```
    
- Partition: Lomuto
    - always pick arr[high] as the pivot
        
        - i + 1 as the position of the pivot. all the elements to the left < pivot
            
            ```C++
            int partition(vector<int>& arr, int low, int high) {
              
                // Choose the pivot
                int pivot = arr[high];
                int i = low - 1;
            
                for (int j = low; j <= high - 1; j++) {
                    if (arr[j] < pivot) {
                        i++;
                        swap(arr[i], arr[j]);
                    }
                }
                
                // Move pivot after smaller elements and
                // return its position
                swap(arr[i + 1], arr[high]);  
                return i + 1;
            }
            ```
            
        
        - i as the position of the pivot. all the elements to the left < pivot
            
            ```C++
            int partition(vector<int>& arr, int low, int high) {
              
                // Choose the pivot
                int pivot = arr[high];
              
                int i = low;
            
                for (int j = low; j <= high - 1; j++) {
                    if (arr[j] < pivot) {
                        swap(arr[i], arr[j]);
                        i++;
                    }
                }
                
                swap(arr[i], arr[high]);  
                return i;
            }
            ```
            
    - always pick arr[low] as the pivot
        
        - i - 1 as the position of the pivot. All elements to the right > pivots.
            
            ```C++
            int partition (vector<int>& arr, int low, int high) {
                // first element as pivot
                int pivot = arr[low];
                int i = high + 1;
                for (int j = high; j > low; j--) {
                    if (arr[j] > pivot){
            		        i--;
                        swap(arr[i], arr[j]);
                    }
                }
                swap(arr[i - 1], arr[low]);
                return i - 1;
            }
            ```
            
        
        - i as the position of the pivot. All elements to the right > pivots
            
            ```C++
            int partition (vector<int>& arr, int low, int high) {
                // first element as pivot
                int pivot = arr[low];
                int i = high;
                for (int j = high; j > low; j--) {
                    if (arr[j] > pivot){
                        swap(arr[i], arr[j]);
                        i--;
                    }
                }
                swap(arr[i], arr[low]);
                return i;
            }
            ```
            
- Ideal Case
    
    Cut the interval into two parts everytime.
    
    ![[Untitled.png]]
    
    Time: $O(n\log n)$﻿
    
    Space: $O(\log n)$﻿
    
- Extreme Case
    
    pivot is at the end every time.
    
    ![[_attachments/Untitled 2.jpeg|Untitled 2.jpeg]]
    
    Time: $O(n^2)$﻿
    
    Space: $O(n)$