# 删除排序链表中的重复元素 II

### 题目

给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 *没有重复出现* 的数字。

**示例 1:**

```
输入: 1->2->3->3->4->4->5
输出: 1->2->5
```

**示例 2:**

```
输入: 1->1->1->2->3
输出: 2->3
```



### 解法

注意本题是要将有重复的都删掉，那么我们在遍历的时候需要记录当前检查节点的前一个节点。然后要做的就是在遍历的时候跳过重复的那一段节点即可。

```c++
// 执行用时 :8 ms, 在所有 cpp 提交中击败了94.28%的用户
// 内存消耗 :9.1 MB, 在所有 cpp 提交中击败了55.01%的用户

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* fakeHead = new ListNode(0);
        fakeHead->next = head;

        ListNode* temp = fakeHead;

        while(temp && temp->next){
            ListNode* temp2 = temp->next;
            
            if(temp2->next && temp2->next->val == temp2->val){
                int duplicateVal = temp2->val;
                while(temp2 && temp2->val == duplicateVal){
                    temp2 = temp2->next;
                }
                temp->next = temp2;
            }else{
                temp = temp->next;
            }
        }
        
        return fakeHead->next;
    }
};
```

