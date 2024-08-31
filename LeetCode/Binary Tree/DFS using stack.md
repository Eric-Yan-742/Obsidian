- 用**栈**去模拟递归
- LeetCode 144 (preorder)
    
    中左右
    
    > [!info] Binary Tree Preorder Traversal - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/binary-tree-preorder-traversal/description/](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)  
    
    ```Java
    class Solution {
        public List<Integer> preorderTraversal(TreeNode root) {
            Stack<TreeNode> st = new Stack<>();
            List<Integer> ls = new ArrayList<>();
            st.push(root);
            while(st.size() != 0) {
                TreeNode cur = st.pop();
                if(cur == null) {
                    continue;
                }
                ls.add(cur.val); // 先check中
                st.push(cur.right); //先压右，这样右后出来
                st.push(cur.left); //再压左
            }
            return ls;
        }
    }
    ```
    
- LeetCode 145 (Postorder)
    
    左右中
    
    > [!info] Binary Tree Postorder Traversal - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/binary-tree-postorder-traversal/](https://leetcode.com/problems/binary-tree-postorder-traversal/)  
    
    ```Java
    class Solution {
        public List<Integer> postorderTraversal(TreeNode root) {
            Stack<TreeNode> st = new Stack<>();
            List<Integer> ls = new ArrayList<>();
            st.push(root);
            while(st.size() != 0) {
                TreeNode cur = st.pop();
                if(cur == null) {
                    continue;
                }
                ls.add(cur.val); //中
                st.push(cur.left); //右
                st.push(cur.right); //左
            }
            Collections.reverse(ls); //中左右反转就是左右中，也就是postorder
            return ls;
        }
    }
    ```
    
- LeetCode 94 (inorder)
    
    左中右
    
    > [!info] Binary Tree Inorder Traversal - LeetCode  
    > Can you solve this real interview question?  
    > [https://leetcode.com/problems/binary-tree-inorder-traversal/](https://leetcode.com/problems/binary-tree-inorder-traversal/)  
    
    - 一旦X的右为空，根据左中右的顺序，说明以X为根节点的子树都访问了，随即就要去弹出栈中下一个元素（如果还有元素的话）
    
    ```Java
    class Solution {
        public List<Integer> inorderTraversal(TreeNode root) {
            List<Integer> ls = new ArrayList<>();
            Stack<TreeNode> st = new Stack<>();
            TreeNode cur = root;
            while(!(cur == null && st.size() == 0)) {//指针和栈都为空时循环结束
                if(cur != null) { 
                    st.push(cur);
                    cur = cur.left; //不为空一路向左
                } else { //指针为空说明触底了，左节点为空访问中，右节点触底就会访问父节点
                    cur = st.pop(); 
                    ls.add(cur.val);
                    cur = cur.right;
                }
            }
            return ls;
        }
    }
    ```