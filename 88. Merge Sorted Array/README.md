# 合并两个有序数组

### 题目

给定两个有序整数数组 *nums1* 和 *nums2*，将 *nums2* 合并到 *nums1* 中*，*使得 *num1* 成为一个有序数组。

**说明:**

- 初始化 *nums1* 和 *nums2* 的元素数量分别为 *m* 和 *n*。
- 你可以假设 *nums1* 有足够的空间（空间大小大于或等于 *m + n*）来保存 *nums2* 中的元素。

**示例:**

```
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```



### 题解

#### 解法一

每个插入一个都移动数组

```javascript
// 执行用时 :60 ms, 在所有 JavaScript 提交中击败了92.63%的用户
// 内存消耗 :34.4 MB, 在所有 JavaScript 提交中击败了46.47%的用户

var merge = function(nums1, m, nums2, n) {
    let j = 0;
    for(let i = 0; i < n; i++){
        while(j < m + i && nums1[j] <= nums2[i])
            j++;
        for(let k = m + i; k > j; k--){
            nums1[k] = nums1[k - 1];
        }
        nums1[j++] = nums2[i];
    }
};
```

#### 解法二

双指针，从前往后，需要把数组1的数据先保存到另外的地方

时间复杂度：O(m+n)，空间复杂度：O(m)

#### 解法三

双指针，从后往前，由于数组1后面本来就是空的，因此从后往前的方式使得空间复杂度降至O(1)

```javascript
// 执行用时 :60 ms, 在所有 JavaScript 提交中击败了92.63%的用户
// 内存消耗 :34.2 MB, 在所有 JavaScript 提交中击败了55.78%的用户

var merge = function(nums1, m, nums2, n) {
    let p1 = m - 1, p2 = n - 1, p = m + n - 1;
    while(p2 >= 0){
        if(nums1[p1] > nums2[p2]){
            nums1[p--] = nums1[p1--];
        }else{
            nums1[p--] = nums2[p2--];
        }
    }
};
```

