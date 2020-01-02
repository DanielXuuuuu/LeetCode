# 最长回文子串

### 题目

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```



### 题解

#### 解法一

这是我最开始自己写的解法，利用了回文串的对称特性。

...太菜了，代码又臭又长。

```c++
// 执行用时 :28 ms, 在所有 cpp 提交中击败了80.03%的用户
// 内存消耗 :12.7 MB, 在所有 cpp 提交中击败了53.14%的用户

class Solution {
public:
    string longestPalindrome(string s) {

        int len = s.size(),  maxLen = 1;
        string subStr = s.substr(0, 1);
        for(int i = 0; i < len; i++){
            int k = 1, currentLen = 1;
            
            // 单数情况
            while(i - k >= 0 && i + k < len){
                if(s[i - k] == s[i + k]){
                    currentLen += 2;
                    k++;
                }else{
                    break;
                }
            }
            if(currentLen > maxLen){
                maxLen = currentLen;
                subStr = s.substr(i - k + 1, maxLen);
            }

            // 双数情况
            if(i - 1 >= 0 && s[i - 1] == s[i]){
                currentLen = 2;
                k = 1;
                while(i - k - 1 >= 0 && i + k < len){
                    if(s[i - k - 1] == s[i + k]){
                        currentLen += 2;
                        k++;
                    }else{
                        break;
                    }
                }
                if(currentLen > maxLen){
                    maxLen = currentLen;
                    subStr = s.substr(i - k, maxLen);
                }
            }else if(i + 1 < len && s[i + 1] == s[i]){
                currentLen = 2;
                k = 1;
                while(i - k >= 0 && i + k + 1 < len){
                    if(s[i - k] == s[i + k + 1]){
                        currentLen += 2;
                        k++;
                    }else{
                        break;
                    }
                }
                if(currentLen > maxLen){
                    maxLen = currentLen;
                    subStr = s.substr(i - k + 1, maxLen);
                }
            }
        }
        return subStr;
    }
};
```

### 解法二

动态规划。

尝试用动态规划，用时反而更久了，可能是我写的有问题。

```c++
// 执行用时 :268 ms, 在所有 cpp 提交中击败了20.78%的用户
// 内存消耗 :12.8 MB, 在所有 cpp 提交中击败了52.95%的用户

class Solution {
public:
    string longestPalindrome(string s) {
        if(s == "")
            return "";
            
        int dp[1001][1001];
        // 初始化
        memset(dp, 0, sizeof(dp));
        for(int i = 0; i < s.size(); i++){
            dp[i][i] = 1;
        }
        for(int i = 0; i < s.size() - 1; i++){
            if(s[i] == s[i + 1]){
                dp[i][i + 1] = 1;
            }
        }

        for(int i = 3; i <= s.size(); i++){
            for(int j = 0; j < s.size() - i + 1; j++){
                dp[j][j + i - 1] = dp[j + 1][j + i - 1 - 1] && s[j] == s[j + i - 1];
            }
        }

        int max = 0, left = 0, right = 0;
        for(int i = 0; i < s.size(); i++){
            for(int j = 0; j < s.size(); j++){
                if(dp[i][j] && j - i + 1 > max){
                    max = j - i + 1;
                    left = i;
                    right = j;
                }
            }
        }

        return s.substr(left, max);
    }
};
```

### 解法三

TODO：Manacher算法