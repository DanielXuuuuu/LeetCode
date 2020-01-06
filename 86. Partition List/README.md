# 分隔链表

### 题目

给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

示例:

```/
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```



### 题解

遍历过程中修改链表指针

```c++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode *temp = head, *p1 = NULL, *p2 = NULL;
        ListNode *firstLess = NULL, *firstNotLess = NULL;

        while(temp != NULL){
            if(temp->val < x){
                if(firstLess == NULL){
                    firstLess = temp;
                    p1 = firstLess;
                }else{
                    p1->next = temp;
                    p1 = p1->next;
                }
            }else{
                if(firstNotLess == NULL){
                    firstNotLess = temp;
                    p2 = firstNotLess;
                }else{
                    p2->next = temp;
                    p2 = p2->next;
                }
            }
            temp = temp->next;
        }
        if(p2 != NULL)
            p2->next = NULL;
        if(p1 != NULL)
            p1->next = firstNotLess;
        
        if(firstLess)
            return firstLess;
        else
            return firstNotLess;
        
    }
};
```



