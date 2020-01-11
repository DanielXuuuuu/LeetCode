# 盛最多水的容器

### 题目

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![img](assets/question_11-20200111091505311.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**示例:**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```



### 题解

#### 解法一

暴力法，遍历所有可能的边的组合。

时间复杂度：O(n2)

空间复杂度：O(1)

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int max = 0;
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                int temp = (height[i] < height[j] ? height[i] : height[j]);
                max = (temp * (j - i) > max ? temp * (j - i) : max);
            }
        }
        return max;
    }
};
```



#### 解法二

双指针法，做法如下：

+ 首先将左右指针都指向两个顶点
+ 然后高度小的指针向高度大的指针移动

原理：因为面积总是受制于短的边以及两点的距离。那么，当我们首先将两个指针指向首尾两点时，此时是两点距离最大的情况，而向内移动必然会减少距离。若此时我们移动的是较长高度对应的那个指针，那么，由于短边没有移动，此时就算我们得到了更大的高度也没用，甚至有可能得到更小的，那么面积会进一步减小。因此，我们对于高度小的指针进行移动，虽然两点距离变小了，但是期望找到一个比短边更高的点，那么面积还是会有增大的可能性的。

时间复杂度：O(n)

空间复杂度：O(1)

（这题和在排序数组中寻找两个数使其和等于一个定值的思路类似）

```c++
// 执行用时 :16 ms, 在所有 C++ 提交中击败了90.86%的用户
// 内存消耗 :9.8 MB, 在所有 C++ 提交中击败了79.87%的用户

class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0, right = height.size() - 1;
        int res = 0;

        while(right > left){
            if(height[left] <= height[right]){
                res = (right - left) * height[left] > res ? (right - left) * height[left] : res;
                left++;
            }else{
                res = (right - left) * height[right] > res ? (right - left) * height[right] : res;
                right--;
            }
        }
        return res;
    }
};
```

