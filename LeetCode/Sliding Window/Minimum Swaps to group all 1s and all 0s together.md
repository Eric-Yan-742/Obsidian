- Tiktok 25summer Aug 17-21
    
    ![[_attachments/image 9.png|image 9.png]]
    
- Note [[1151. Minimum Swaps to Group All 1's Together]] is different from this one. The solution of that problem will not work for case like 01110. Still cound’t figure out one thing. If we consider both cases of 0s and 1s and take the max # steps of 2 cases, why it still doesn’t work for this problem
- One potential solution: One way to sort the array is to move all the 0s to the start or to the end. Therefore, we need to count the number of swaps needed to move all 0s to the start or to the end of the array. Start by counting the number of 0s `**zeroes**` in the array. Then form two windows of size `**zeroes**` - one at the start and one at the end of the array. For each window, the number of swaps needed to move all zeroes to this window = the number of 1s in the window. Return the minimum swaps of the two windows.

```Java
def getOptimalContentStorage(tiktokStorage: List[int]) -> int:
  n = len(tiktokStorage)
  zeroes = tiktokStorage.count(0)
  if zeroes == 0 or zeroes == n:
    return 0
  
  res = n
  fwdSwaps, bckSwaps = 0, 0

  for i in range(zeroes):
    if tiktokStorage[i] == 1:
      fwdSwaps += 1

  for i in range(n-1, n-1-zeroes, -1):
    if tiktokStorage[i] == 1:
      bckSwaps += 1
  
  res = min(fwdSwaps, bckSwaps)
  return res
```