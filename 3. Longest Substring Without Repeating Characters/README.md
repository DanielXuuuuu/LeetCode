# 无重复字符的最长子串

### 题目

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

##### 示例 1

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

##### 示例 2

> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

##### 示例 3

> 输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。



### 解法

#### 一、暴力法

最简单的解法，使用两层循环。外层循环从头遍历到尾，内层循环检查从当前位置起，无重复字符的子串长度，若出现重复字符，则跳出循环。

```c++
// 执行用时:36 ms, 在所有 cpp 提交中击败了36.25%的用户
// 内存消耗:9.1 MB, 在所有 cpp 提交中击败了92.00%的用户

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s == "")
            return 0;

        int max = 1, temp, len = s.length();
        int a[256];
        memset(a, 0, sizeof(a));

        for(int i = 0; i < len; i++){
            a[s[i]] = 1;
            temp = 1;
            int j;
            for(j = i + 1; j < len; j++){
                if(a[s[j]] != 0){
                    break;
                }
                else{
                    a[s[j]] = 1;
                    max = (++temp > max ? temp : max);
                }
            }
            memset(a, 0, sizeof(a));
        }
        return max;
    }
};
```

