# 平衡二叉树判断

### 题目

给定一个二叉树，判断它是否是高度平衡的二叉树。

一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

##### 示例 1

给定二叉树 [3,9,20,null,null,15,7]

```/
	3
 / \
9  20
  /  \
 15   7
```

返回 true 

##### 示例 2

给定二叉树 [1,2,2,3,3,null,null,4,4]

```/
      1
     / \
    2   2
   / \
  3   3
 / \
4   4
```
返回 false 。



### 解法

#### 一、暴力法

直接遍历树的所有节点，对于每个节点判断其左右两边高度。

本来以为这种解法会很慢甚至超时，结果速度却还可以，但是内存花费比较大，应该主要是对于每个节点递归都求左右两边高度导致的。

```c++
// 执行用时 :12 ms, 在所有 cpp 提交中击败了93.21%的用户
// 内存消耗 :17.7 MB, 在所有 cpp 提交中击败了5.09%的用户

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(root == NULL){
            return true;
        }

        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* temp = q.front();
            q.pop();
            int left_depth = 0, right_depth = 0;
            if(temp->left){
                left_depth = depth(temp->left);
                q.push(temp->left);
            }
            if(temp->right){
                right_depth = depth(temp->right);
                q.push(temp->right);
            }
            if(abs(left_depth - right_depth) > 1){
                return false;
            }
        }
        return true;
    }

    int depth(TreeNode * root){
        if(root == NULL)
            return 0;
        return 1 + max(depth(root->left), depth(root->right));
    }
};
```

