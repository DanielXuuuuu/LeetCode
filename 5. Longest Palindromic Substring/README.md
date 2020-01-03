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

这是我最开始自己写的解法，思想和中心扩展法相同，利用了回文串的对称特性。

...但是太菜了，代码又臭又长。

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

**Manacher算法**

核心的思想还是中心扩展法，但是由于在最基本的中心扩展法中，回文子串长度会出现奇数和偶数的情况，且经常需要对于同一个位置遍历好几遍，因此效率不高。Manacher算法正是解决了这一问题，把时间复杂度降到了O(n)。

具体实现方法之后补充。可见[参考](https://segmentfault.com/a/1190000003914228)

```c++
// 执行用时 :16 ms, 在所有 cpp 提交中击败了91.09%的用户
// 内存消耗 :9.4 MB, 在所有 cpp 提交中击败了74.15%的用户

class Solution {
public:
    string longestPalindrome(string s) {
        string temp(s);
        // 插入#
        for(int i = 0; i <= temp.size(); i+=2){
            temp.insert(i, 1, '#');
        }

        int maxRight = 0, pos = 0, maxLen = 0, maxPos = 0;
        int RL[2005];
        RL[0] = 1;

        for(int i = 0; i < temp.size(); i++){
            if(i < maxRight){
                RL[i] = min(RL[2 * pos - i], maxRight - i + 1);
            }else{
                RL[i] = 1;
            }

            while(i - RL[i] >= 0 && i + RL[i] < temp.size() && temp[i - RL[i]] == temp[i + RL[i]]){
                RL[i]++;
            }

            if(RL[i] + i - 1 > maxRight){
                maxRight = RL[i] + i - 1;
                pos = i;
            }

            if(RL[i] > maxLen){
                maxLen = RL[i];
                maxPos = i;
            }
        }

        int originPos = (maxPos - maxLen + 1) / 2;
        return s.substr(originPos, maxLen - 1);
    }
};
```

