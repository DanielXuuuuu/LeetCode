# 验证二叉搜索树

### 题目

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含**小于**当前节点的数。
- 节点的右子树只包含**大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1:**

```
输入:
    2
   / \
  1   3
输出: true
```

**示例 2:**

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```



### 题解

错误解法：简单判断左右子树，然后判断根和左右两个子树的根的大小。反例如下，其中3小于5：

```/
输入:
    5
   / \
  1   6
     / \
    3   7
```



#### 解法一

判断中序遍历为升序

```c++
// 执行用时 :16 ms, 在所有 C++ 提交中击败了77.06%的用户
// 内存消耗 :21.2 MB, 在所有 C++ 提交中击败了5.16%的用户

class Solution {
public:
    vector<int> temp;
    bool isValidBST(TreeNode* root) {
        if(root == NULL){
            return true;
        }
        // 左子树
        if(!isValidBST(root->left)){
            return false;
        }
        // 当前遍历到的根节点，判断是否是升序
        if(temp.size() != 0 && root->val <= temp[temp.size() - 1]){
            return false;
        }
        temp.push_back(root->val);
        // 右子树
        if(!isValidBST(root->right)){
            return false;
        }

        return true;
    }
};
```

