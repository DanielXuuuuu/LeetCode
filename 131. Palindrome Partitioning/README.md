# 分割回文串

### 题目

给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回 *s* 所有可能的分割方案。

**示例:**

```
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```



### 题解

#### 解法一

比较暴力的递归+回溯解法。对于一个字符串，首先将其分割为左右两个字串，一共有`len(str)`种分法。然后判断左子串是否是回文串：若不是就跳过；若是，则将左子串记录下来，然后对右子串做进一步的递归分割。

```c++
// 执行用时 :68 ms, 在所有 C++ 提交中击败了47.89%的用户
// 内存消耗 :24.4 MB, 在所有 C++ 提交中击败了53.40%的用户

class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> temp;
        helper(res, temp, s, 0);
        return res;
    }

    void helper(vector<vector<string>>& res, vector<string>& temp, string s, int start){
        if(start == s.size()){
            res.push_back(temp);
            return;
        }

        for(int i = 1; i <= s.size() - start; i++){
            string left = s.substr(start, i);
            if(check(left)){
                temp.push_back(left);
                helper(res, temp, s, start + i);
                temp.pop_back();
            }
        }
    }

    bool check(string str){
        for(int i = 0, j = str.size() - 1; i <= j; i++, j--){
            if(str[i] != str[j]){
                return false;
            }
        }
        return true;
    }
};
```

