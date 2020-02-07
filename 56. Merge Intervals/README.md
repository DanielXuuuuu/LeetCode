# 合并区间

### 题目

给出一个区间的集合，请合并所有重叠的区间。

**示例 1:**

```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2:**

```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```



### 题解

由于所给区别集合可能不像示例中那样有序，因此首先根据区间左边界进行排序，然后判断区间是否重叠即可。

```javascript
// 执行用时 :92 ms, 在所有 JavaScript 提交中击败了55.18%的用户
// 内存消耗 :36.8 MB, 在所有 JavaScript 提交中击败了77.80%的用户

/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    if(intervals.length === 0) 
        return [];
    intervals.sort((a, b) => a[0] - b[0]);
    res = [intervals[0]];
    for(let i = 1; i < intervals.length; i++){
        if(res[res.length - 1][1] >= intervals[i][0]){
            res[res.length - 1][1] = Math.max(res[res.length - 1][1], intervals[i][1]);
        }else{
            res.push(intervals[i]);
        }
    }
    return res;
};


// JS 数组排序，sort: a-b升序，b-a降序
```

