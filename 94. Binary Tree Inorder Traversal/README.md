# 二叉树中序遍历

### 题目

给定一个二叉树，返回它的中序遍历。

示例:

```/
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

进阶: 递归算法很简单，你可以通过迭代算法完成吗？



### 题解

#### 解法一

递归，先递归左子树，再输出自己的值，再递归右子树

```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        helper(res, root);
        return res;
    }

    void helper(vector<int>& res, TreeNode* root){
        if(root == NULL){
            return;
        }
        if(root->left)
            helper(res, root->left);
        res.push_back(root->val);
        if(root->right)
            helper(res, root->right);
    }
};
```





#### 解法二

迭代法，利用指针和栈。过程如下：

+ 首先指针一路遍历到最左边的叶节点，过程中将遇到的所有节点都压栈。
+ 此时得到的节点不再具有左子树，因此我们输出这个节点的值
+ 然后把指针指向它的右节点，重复上述操作，即：若右节点存在的话，对于以这个右节点为根的子树，我们也需要首先得到这棵子树的最左边的叶节点；若不存在，我们就直接去访问栈顶元素，此时以这个栈顶元素为根的子树的左子树已经被我们访问完毕了

```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> s;
        TreeNode* p = root;

        while(p || !s.empty()){
            if(p){
                s.push(p);
                while(p->left){
                    p = p->left;
                    s.push(p);
                }
            }
            p = s.top();
            res.push_back(p->val);
            s.pop();
            p = p->right;
        }
        return res;
    }
};
```

