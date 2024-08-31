- LeetCode 530
    
    > [!info] Minimum Absolute Difference in BST - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/minimum-absolute-difference-in-bst/](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)  
    
- 对于一个有序序列，最小差值一定是两个相邻的数，所以只用比较相邻
- 中序遍历转变成有序序列，暴力解法就是把二叉树转换成有序数组，然后在数组中找最小差值。
- 双指针法（不需要额外定义数组）
    
    ```Java
    class Solution {
        //result设为int最大值，这样result一开始会比任何差值都要大
        //如果result初始值恰巧比最小差值还小，那result永远不会被改
        //最后返回的是一个错误的result的初始值。但int的最大值避免了
        //这一点
        int result = Integer.MAX_VALUE;
        TreeNode pre = null;
        public int getMinimumDifference(TreeNode root) {
            traversal(root);
            return result;
        }
        private void traversal(TreeNode node) {
            //Base Case: 如果为空直接打断，不影响result和pre
            if(node == null) {
                return;
            }
            //左
            traversal(node.left);
            //中
            //对于第一个检验的元素（左下角），它自己一个没有差值，所以用pre!=null跳过它
            if(pre != null) {
                result = Math.min(result, Math.abs(node.val - pre.val));
            }
            //让pre指向当前元素，为下一次比较做准备
            pre = node;
            //右
            traversal(node.right);
        }
    }
    ```
    
- 这题不可避免的用了MAX_VALUE。我的想法是上一题仅靠比较每一组相邻的元素就能得出结论，因此单独双指针的就够用。但这题除了双指针之外，必须要一个变量来记录直到现在最小的差值。因为我们需要这样一个变量，它就必须有初始值。初始值在这种情况下只能是MAX_VALUE。
    - 其实如果是循环遍历一个数组的话，可以让result的值等于第一个和第二个的差值（第一个差值）。但这种在递归中实现太困难了。

```C++
class Solution {
private:
    int minDiff;
    TreeNode* pre;
    void inorder(TreeNode* cur) {
        if(cur == nullptr) return;
        inorder(cur->left);

        // if it's not the first node, update minDiff
        if(pre != nullptr) {
            // no need to use absolute value
            // because for an increasing ordered array
            // next element is always bigger or equal to this element
            minDiff = min(minDiff, cur->val - pre->val);
        }
        pre = cur;

        inorder(cur->right);
    }
public:
    int getMinimumDifference(TreeNode* root) {
        minDiff = INT_MAX;
        pre = nullptr;

        inorder(root);
        return minDiff;
    }
};
```