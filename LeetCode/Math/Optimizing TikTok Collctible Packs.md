Tiktok 25summer 8.10 - 8.15 OA Q6

![[_attachments/image 10.png|image 10.png]]

第一题简单来说就是往数组里加数，使得数组的所有数都存在一个除了1以外的公因数， 这里有些人可能会简单的用2和3来当公因数然后取最小值，但是我们不仅需要考虑2，3.其它数也有可能是公因数， 比如 [5,5,5,5,10]这个数组可以直接使用5作为公因数而不添加任何数。好在题干中给的数值取值范围只有1到500，所以我们即使遍历所有可能的公因数也不会超时。我们可以取数组中最大的那个数作为公因数的上限进行遍历，计算所有公因数所需要加的数然后取最小值。

  

```C++
int cardPackets(vector<int>& cardTypes) {
	int maxVal = cardTypes[0];
	int res = INT_MAX;
	for (int i = 1; i < cardTypes.size(); i++) {
		if (cardTypes[i] > maxVal) maxVal = cardTypes[i];
	}
	for(int i = 2; i <= maxVal; i++) {
		int add = 0;
		for (int j = 0; j < cardTypes.size(); j++) {
			// Use the property of modulus to calculate 
			// # of cards to add for each type in O(1)
			if (cardTypes[j] % i != 0)
				add += i - (cardTypes[j] % i);
		}
		res = min(add, res);
	}
	return res;
}
```

- A new bullet point

	```cpp
	int cardPackets(vector<int>& cardTypes) {
		int maxVal = cardTypes[0];
		int res = INT_MAX;
		for (int i = 1; i < cardTypes.size(); i++) {
			if (cardTypes[i] > maxVal) maxVal = cardTypes[i];
		}
		for(int i = 2; i <= maxVal; i++) {
			int add = 0;
			for (int j = 0; j < cardTypes.size(); j++) {
				// Use the property of modulus to calculate 
				// # of cards to add for each type in O(1)
				if (cardTypes[j] % i != 0)
					add += i - (cardTypes[j] % i);
			}
			res = min(add, res);
		}
		return res;
	}
	```
