- 巨简单，记住base case是 `node == null` 就行了
- LeetCode 144 (preorder)
    
    > [!info] Binary Tree Preorder Traversal - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/binary-tree-preorder-traversal/](https://leetcode.com/problems/binary-tree-preorder-traversal/)  
    
    ```C++
    class Solution {
    public:
        void traversal(TreeNode* cur, vector<int>& vec) {
            if (cur == NULL) return;
            vec.push_back(cur->val);    // 中
            traversal(cur->left, vec);  // 左
            traversal(cur->right, vec); // 右
        }
        vector<int> preorderTraversal(TreeNode* root) {
            vector<int> result;
            traversal(root, result);
            return result;
        }
    };
    ```
    
    ```Java
    /**
     * Definition for a binary tree node.
     * public class TreeNode {
     *     int val;
     *     TreeNode left;
     *     TreeNode right;
     *     TreeNode() {}
     *     TreeNode(int val) { this.val = val; }
     *     TreeNode(int val, TreeNode left, TreeNode right) {
     *         this.val = val;
     *         this.left = left;
     *         this.right = right;
     *     }
     * }
     */
    class Solution {
        public List<Integer> preorderTraversal(TreeNode root) {
            List<Integer> list = new ArrayList<>();
            traverseHelper(list, root);
            return list;
        }
        private void traverseHelper(List<Integer> list, TreeNode node) {
            if(node == null)
                return;
            list.add(node.val);
            traverseHelper(list, node.left);
            traverseHelper(list, node.right);
        }
    }
    ```
    
- LeetCode 145 (postorder)
    
    > [!info] LeetCode - The World's Leading Online Programming Learning Platform  
    > Level up your coding skills and quickly land a job.  
    > [https://leetcode.com/problems/binary-tree-postorder-traversal/](https://leetcode.com/problems/binary-tree-postorder-traversal/)  
    
    ```C++
    void traversal(TreeNode* cur, vector<int>& vec) {
    	if (cur == NULL) return;
    	traversal(cur->left, vec);  // 左
    	traversal(cur->right, vec); // 右
    	vec.push_back(cur->val);    // 中
    }
    ```
    
    ```Java
    class Solution {
        public List<Integer> postorderTraversal(TreeNode root) {
            List<Integer> list = new ArrayList<>();
            traverseHelper(list, root);
            return list;
        }
        private void traverseHelper(List<Integer> list, TreeNode node) {
            if(node == null)
                return;
            traverseHelper(list, node.left);
            traverseHelper(list, node.right);
            list.add(node.val);
        }
    }
    ```
    
- LeetCode 94 (inorder)
    
    > [!info] Binary Tree Inorder Traversal - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/binary-tree-inorder-traversal/](https://leetcode.com/problems/binary-tree-inorder-traversal/)  
    
    ```C++
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        traversal(cur->left, vec);  // 左
        vec.push_back(cur->val);    // 中
        traversal(cur->right, vec); // 右
    }
    ```
    
    ```Java
    class Solution {
        public List<Integer> inorderTraversal(TreeNode root) {
            List<Integer> list = new ArrayList<>();
            traverseHelper(list, root);
            return list;
        }
        private void traverseHelper(List<Integer> list, TreeNode node) {
            if(node == null)
                return;
            traverseHelper(list, node.left);
            list.add(node.val);
            traverseHelper(list, node.right);
        }
    }
    ```